# Technical requirements

For this chapter, you need to have Docker installed on your system. For a detailed explanation, see Chapter 6, in which the complete installation of Docker is covered. We are going to use Docker to spin up instances for the integration tests. The first part of this chapter is doable without Docker; however, to get the most value out of this chapter, it is important to have Docker installed.

You can find all the code examples from this chapter on GitHub: https://github.com/PacktPublishing/Cloud-Native-Development-and-Migration-to-Jakarta-EE/tree/chapter-5. The repository contains all the examples from this chapter.

# The impact of testing on your migration

Tests are a great tool to measure if your migration was a success or if you still need to fix some parts of the project you are migrating. When all the tests are working correctly, it means that all the functionality those tests cover works just as before the migration. There is also the possibility that some tests started failing during the migration, meaning that some parts of the migration broke the functionality.

If the project you are migrating already has enough tests to cover the functionality, you can skim this chapter to see if you see new things you would like to incorporate in your testing strategy or see an alternative way of testing a Java application. To migrate an application, it is not necessary to overhaul the current testing strategy.

If you are migrating a project with little to no tests, adding tests can help you immensely as the test will indicate if the project keeps working. This chapter covers one of many ways to write tests and have test coverage for the project you are migrating. There are a lot more libraries out there that are worth considering for testing your application, such as Arquillian, Jakarta REST client, and MicroProfile REST client.

# Measuring code coverage of the project

Having tests is great, but do you know how much of your project is covered by the tests inside the project? That is where test coverage comes in. Test coverage tells you how much production code is covered by tests. This tells you what functionality is covered by tests and what functionality has to be tested manually.

Measuring test coverage is very easy when you use IntelliJ. All you have to do is navigate to a test class. Inside a test class, you can either right-click on the class to run the tests with coverage or you can run the tests with coverage from the menu options, as in the following screenshot: