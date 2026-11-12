15. The composer.lock file is the state file for installed dependencies. The composer.phar file is the executable file of Composer, downloaded to be run with the PHP configured in Eclipse.
16. Since the composer.lock file is created, you must use the **Update dependencies** button for updating the components installed via Composer:

![](bbox=[0.235, 0.195, 0.725, 0.39])

Figure 3.14 – The Update dependencies button

Great! Finally, we have an instance of PHPUnit installed into our project.

> **Tip**
> It is recommended that you close and open the project to ensure that autocomplete options will consider the installed dependencies.

So, we have learned how to create a PHP project structure from a use case and how to install the dependencies of a test framework in Eclipse. In the next section, we will start to develop our use case further, aided by tests.

# # Developing our use case with TDD

From now on, we are going to start TDD. This is an approach popularized by Kent Beck, who is one of the signatories of the *Manifesto for Agile Software Development*. Beck states that TDD is a way of programming without fear. When you write tests before you create the code that they test, you are writing requirements as code. A test that can be run anytime tells us whether code meets the requirements anytime. It is not enough to create a code that works. We need to maintain the code in working order after each change. Tests tell us whether a change has stopped the code from working.