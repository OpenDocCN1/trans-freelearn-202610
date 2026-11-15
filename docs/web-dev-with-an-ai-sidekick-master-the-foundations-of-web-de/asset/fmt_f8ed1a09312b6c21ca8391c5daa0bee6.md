
## More flexible test organization

A `pytest`-style test might look like this:

```python
def test_survey_str(db, django_user_model):
    user = django_user_model.objects.create_user(
        username="mark",
        password="password123"
    )
    survey = Survey.objects.create(
        title="Favorite Editor",
        owner=user
    )
    assert str(survey) == "Favorite Editor"
```

Note the following about the preceding code:

* `db` is a fixture that enables database access
* Assertions use plain `assert` instead of `self.assertEqual`

`pytest-django` integrates `pytest` with Django's settings and test database.

Many Django teams use `pytest` with `pytest-django`.

**Prompt**: Why do many professional Django teams prefer `pytest` over `unittest`? Please include a comparison of the same Django test written in `unittest` style and `pytest` style.

Now let's move on from unit testing to integration testing.

# Integration testing Django applications

**Integration tests** sit in the middle of your test suite. They are not trying to prove every micro-detail of a function (unit tests), and they are not trying to drive a real browser (end-to-end UI tests). Instead, they verify that multiple Django layers cooperate correctly: URL routing, view logic, templates, middleware, authentication, sessions, and database reads and writes.

Django makes this style of testing fairly painless because the built-in test runner automatically creates a temporary test database, wraps each test in a transaction, and provides a test client that can make request-like calls against your Django project without starting a browser.

## What you test at the integration level

A good integration test suite answers questions like these:

* If I request a named URL, do I hit the expected view and get the correct HTTP response? (URLs + views)
* If I am not logged in, do protected pages redirect me to the login page? If I am logged in, do I get access? (authentication flow)
* Do middleware hooks actually run and alter requests or responses the way the live site will? (middleware in the request/response pipeline)
* Do views correctly create, update, and query database rows, and does the response reflect those changes? (database interactions)

**Prompt**: What are the most common mistakes developers make when writing integration tests?

## TestCase and the test client

Django's `django.test.TestCase` gives you a database that is set up for each test run, and it provides a `self.client` attribute you can use to perform requests like `GET` and `POST`. The client can also follow redirects and record the redirect chain when you pass `follow=True`.

A typical pattern is:

1. Create the database state you need (users, surveys, questions, responses).
2. Make an HTTP request with `self.client.get(...)` or `self.client.post(...)`.
3. Assert status codes, redirects, context data, and database effects.

When you call `self.client.get()` in a Django `TestCase`, Django's test client simulates an HTTP request and runs it through (almost) the entire Django stack without a real browser, as shown in *Figure 12.3*:

![](Images/B34553_12_03.png)

*Figure 12.3: Django integration test request path*

`self.client` runs a request through Django's real request/response cycle including routing, middleware, templates, and the database, all without a browser.

## Example: testing URL routing and a view together

This example assumes you have a survey detail page that shows a published survey.

In the file `tests/test_surveys_integration.py`:

```python
from django.test import TestCase
from django.urls import reverse, resolve
from surveys.views import survey_detail
from surveys.models import Survey

class SurveyDetailIntegrationTests(TestCase):
    @classmethod
    def setUpTestData(cls):
        # setUpTestData runs once for the class (faster than setUp per test)
        cls.survey = Survey.objects.create(
            title="Customer Satisfaction",
            slug="customer-satisfaction",
            is_published=True,
        )

    def test_url_resolves_to_expected_view(self):
        url = reverse("surveys:detail", kwargs={"slug": self.survey.slug})
        match = resolve(url)
        self.assertEqual(match.func, survey_detail)

    def test_detail_page_returns_200_and_includes_title(self):
        url = reverse("surveys:detail", kwargs={"slug": self.survey.slug})
        response = self.client.get(url)
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, "Customer Satisfaction")
```

Note the following about the preceding code:

* `reverse()` ensures you are testing the named route rather than hardcoding URLs, so refactors are safer.
* `resolve()` verifies your `URLconf` points at the correct view.
* `self.client.get()` exercises URL routing + middleware stack + view + template rendering together.

**Warning!** Objects created in `setUpTestData` are shared across all tests in the class. If a test modifies and saves the object, subsequent tests see the modified version. Use `setUpTestData` only for data that tests read but don't change. For mutable test data, use `setUp` instead.

## Example: testing a login-required view

For creator-only pages in TallyApp, a typical rule is: if you are not authenticated, you should be redirected to the login page. Once logged in, you should see the page.

MDN's Django testing tutorial shows this exact pattern with `assertRedirects` for *redirect if not logged in*:

```python
from django.contrib.auth import get_user_model
from django.test import TestCase
from django.urls import reverse

User = get_user_model()

class SurveyCreateAuthIntegrationTests(TestCase):
    @classmethod
    def setUpTestData(cls):
        cls.user = User.objects.create_user(
            username="mark",
            email="mark@example.com",
            password="pass12345",
        )

    def test_redirects_to_login_when_not_authenticated(self):
        url = reverse("surveys:create")
        response = self.client.get(url)
        # If you use Django's login_required decorator, this is the default behavior:
        # redirect to settings.LOGIN_URL with ?next=<original_url>.
        login_url = reverse("login")
        self.assertRedirects(
            response,
            f"{login_url}?next={url}",
            fetch_redirect_response=False,  # avoids making a second request
        )

    def test_returns_200_when_authenticated(self):
        self.client.force_login(self.user)
        url = reverse("surveys:create")
        response = self.client.get(url)
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, "Create survey")
```

Note the following about the preceding code:

* `force_login` is ideal when you are not testing the login form itself, only the access control and behavior once authenticated.
* `assertRedirects` is the most direct way to test redirects in Django's test client.

## Example: testing a full "POST creates rows" flow

Now let's test something closer to real behavior: submit a POST that creates a survey, confirm you are redirected to the new survey page, and confirm the survey exists in the database:
