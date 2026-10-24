It is certainly true that the execution of these tests can take time; in this case, it is possible to schedule their execution at night. But these integration tests are the ones that will guarantee the quality of the application's smooth operation during all stages of delivery, from deployment to production.

However, there is often a very bad practice of disabling the execution of tests in the CI pipeline in case their execution fails. This is often done to avoid the blocking of the complete CI process and thus deliver faster in production. But keep in mind that errors detected by the unit tests, including ones that would have been detected by tests that have been disabled, will be detected in the production stage at some point, and the correction of the failed code will cost more time than if the tests had been enabled during CI.

We have also learned about other types of tests in this book, such as code analysis tests or security tests, which are not to be ignored. The sooner they are integrated into CI/CD pipelines, the more value will be added for maintaining code and securing our application.

To summarize, you should not ignore the implementation of tests in your applications and their integration into CI/CD pipelines, as they guarantee the quality of your application.

After good practices for test integration, I suggest you see what good practices there are for the integration of security in your CI/CD processes.

# # Shifting security left with DevSecOps

As we discussed in Chapter 14, Security in the DevOps Process with DevSecOps, security and compliance analyses must be part of DevOps processes. However, in companies, there is often a lack of awareness among development teams about security rules, and this is why security is implemented too late in DevOps processes.

To integrate security into processes, it is, therefore, necessary to raise awareness among developers of aspects of application code security, but also of the protection of CI/CD pipeline configuration.

In addition, it is also necessary to eliminate the barrier between DevOps and security by integrating security teams more often into the various meetings that bring together Dev and Ops teams, thus ensuring better consistency between developers, operational teams, and also security. Regarding the choice of tools, don't use too many different tools, because the goal is for these tools to also be used by developers and be integrated into CI/ CD pipelines. It is, therefore, necessary to select a few tools that are automated, do not require great knowledge of security, and provide reports for better analysis.