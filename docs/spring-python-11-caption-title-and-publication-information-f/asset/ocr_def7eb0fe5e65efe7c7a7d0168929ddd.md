This makes it easy to plug in our alternative `StubDataAccess`. An important thing to note is how our test code is no longer overriding the `data_access` attribute, or doing any other special steps when creating `WikiService`. Instead, this creation logic has been totally turned over to the IoC container. We can now easily write as many tests as we want with no risk of changes to `WikiService` or `data_access`, provided we continue to rely on the IoC container to handle object creation. We just visit the blueprints of `WikiTestAppConfig` to make any future alterations.

In conclusion of this example, the following diagram shows our current objects as well as potential enhancement to our system. In this possible situation, we have defined multiple data access components. To find out which one is being injected into `WikiService`, we simply look at the relevant container's definition, whether its production or a particular test scenario. Our current system may use **MySQL**, but if a new customer wanted to use **PostGreSQL**, we simply create another variation of our container, and inject the alternate data access object. The same can be said for **Sqlite**. And all of this can be done with no impact to `WikiService`.

![](bbox=[0.17, 0.41, 0.83, 0.64])

# Container versus Context

You may have noticed in this chapter's example how we keep referring to the code that contains our object definitions as a container, and yet, the class used to create it is called `ApplicationContext`. This chapter will clarify the differences between a **context** and a **container**.