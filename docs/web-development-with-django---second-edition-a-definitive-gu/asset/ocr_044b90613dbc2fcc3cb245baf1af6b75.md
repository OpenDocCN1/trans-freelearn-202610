# 14

# Testing Your Django Applications

In the preceding chapters, we focused on building our web application in Django by writing different components such as database models, views, and templates. We did all that to provide our users with an interactive application where they can create a profile and write reviews for the books they have read.

Apart from building and running the application, there is another important aspect of making sure that the application code works the way we expect it to work. This is ensured by a technique called **testing**. In testing, we run the different parts of the web application and check whether the output of the executed component matches the output we expected. If the output matches, we say that the component was tested successfully, while if the output doesn’t match, we say that the component failed to work as intended.

In this chapter, as we go through the different sections, we will learn why testing is important, the different ways to test a web application, and how we can build a strong testing strategy that will help us ensure that the web application we build is robust.

In this chapter, we will cover the following topics:

+   - Importance of testing
+   - Automation testing
+   - Testing in Django
+   - Testing Django models
+   - Testing Django views
+   - Django request factory
+   - Test case classes in Django

Let’s start our journey by first learning about the importance of testing.