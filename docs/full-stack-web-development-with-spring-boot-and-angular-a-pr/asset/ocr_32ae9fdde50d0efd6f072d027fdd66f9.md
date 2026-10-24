# Understanding Spring Boot

We have already discussed an overview of Spring in *Chapter 1, Spring Boot and Angular – The Big Picture*. In this section, we will have a deeper understanding of the essential concepts of Spring Boot in building your backend application, but first, let’s recap what Spring Boot is and its significant advantages.

**Spring Boot** is an open source micro-framework from Pivotal. It is an enterprise-level framework for developers to create standalone applications on **Java Virtual Machines (JVMs)**. Its primary focus is to shorten your code length to make it easier for you to run your application.

The framework extends the Spring Framework, which allows a more opinionated way to configure your applications. In addition, it comes with built-in autoconfiguration capabilities that configure both Spring Framework and third-party packages based on your settings.

Here are the significant advantages of Spring Boot:

+   - **Auto-configuration:** When configuring your Spring Boot application, it downloads all the dependencies needed to run your application.
+   - **Opinionated approach:** Spring Boot uses a narrow approach to installing dependencies based on your application’s needs. Manual configuration is removed as it adds the packages you need for your application.
+   - **Spring starters:** We can choose a list of starter dependencies to define your application’s expected needs during the initialization process. One example is Spring Web, which allows us to initialize a Spring-based web application.

Now, we know what Spring Boot is and its advantages. Let’s now discuss the architecture of Sprint Boot.

# Spring Boot architecture

Spring Boot consists of different layers and classes to process the data and logic in your backend. The four layers and their use are as follows:

+   1. **Presentation/display layer:** The presentation layer is responsible for interpreting JSON parameters as objects. This layer is the upper layer that is also responsible for handling authentication and HTTP requests. After accomplishing JSON translation and authentication, we will now move to the business layer.
+   2. **Business layer:** The business layer, as the name suggests, handles all the business logic in the application. It is composed of service classes that perform authorization and additional validation.
+   3. **Persistence layer:** The persistence layer is mainly responsible for storage logic that converts objects from and to database rows to insert data.
+   4. **Database layer:** The database layer performs **Create, Read, Update, and Delete (CRUD)** operations. The layer can consist of multiple databases.