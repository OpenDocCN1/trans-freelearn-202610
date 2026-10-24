# # Writing Tests in Spring Boot

In the previous chapter, you have learned about the importance of loggers, their concepts, and how they can help developers debug and maintain applications. You have learned about Log4j2, which is a third-party framework for Spring Boot that offers several features such as **Appenders**, **Filters**, and **Markers** that can assist in making log events categorized and formatted for developers. We have also discussed SLF4J, which is an abstraction of logging frameworks that allows us to switch between different frameworks during runtime or at deployment, and lastly, we have implemented and configured the logging frameworks with XML configuration and Lombok.

This chapter will now focus on writing unit tests for our Spring Boot application; we will discuss the most commonly used testing frameworks with Java, JUnit, and AssertJ and implement them in our application. We will also be integrating Mockito with our unit test for mocking objects and services.

In this chapter, we will cover the following topics:

+   - Understanding JUnit and AssertJ
+   - Writing a test
+   - Writing tests in a service using Mockito

# ## Technical requirements

The link to the finished version of this chapter is here: https://github.com/PacktPublishing/Spring-Boot-and-Angular/tree/main/Chapter-09.

# ## Understanding JUnit and AssertJ

After every development of an application, testing will always be the next step, and this is one of the most important tasks before delivering or deploying our application into production for the world. The testing phase is critical for companies, as this ensures the quality and effectiveness of their products.