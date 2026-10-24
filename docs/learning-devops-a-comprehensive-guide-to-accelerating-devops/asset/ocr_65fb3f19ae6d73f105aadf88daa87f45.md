In Chapter 15, Reducing Deployment Downtime, we discussed another good practice that allows deployment in production more frequently, which consists of encapsulating the functionalities of the application in feature flags. These feature flags must also be taken into account when designing the application, as they allow the application to be deployed in the production stage, enabling/disabling its features dynamically without having to redeploy it.

Finally, the implementation of unit tests and the logging mechanism must be taken into account as soon as possible in the development of the application because these allow feedback on the state of the application to be shared very quickly in its deployment cycle.

A DevOps culture involves the implementation of CI/CD pipelines; as we have just seen, this requires changes in the design of applications, with the separation of functionalities to create less-monolithic applications, the implementation of tests, and the addition of a logging system.

After considering good practices for application design, we will explore some good practices for the implementation of CI/CD pipelines.

# # Building a good CI/CD pipeline

In this book, we have dedicated a complete chapter, Chapter 7, Continuous Integration and Continuous Delivery, to the creation of CI/CD pipelines using different tools such as GitLab CI, Jenkins, and Azure Pipelines, in which we have already mentioned the prerequisites for the implementation of CI/CD pipelines.

We also discussed the CI/CD process in Chapter 16, DevOps for Open Source Projects, with some examples of a CI pipeline for open source projects such as Travis CI and GitHub Actions.

Building a good CI/CD pipeline is indeed an essential practice in a DevOps culture and, together with the correct choice of tools, allows for faster deployment and better-quality applications.

One of the best practices for CI/CD pipelines is to set them up as early as the project launch stage. This is especially true for the CI pipeline, which will allow the code (at least the compilation step) to be verified when writing the first lines of code. Then, as soon as the first environment is provisioned, immediately create a deployment pipeline, which will allow the application to be deployed and tested in this environment. The rest of the CI/ CD pipeline process's tasks, such as unit-test execution, can be performed as the project progresses.