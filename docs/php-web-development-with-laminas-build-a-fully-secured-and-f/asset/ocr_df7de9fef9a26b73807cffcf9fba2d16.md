# # Event-Driven Authentication

This chapter covers one of the essential topics for developing secure applications – authentication. Authentication is an identity check. An example of authentication in the physical world is the request for an identity document to get into a building.

In this chapter, you will learn how to implement this checking process with event-oriented programming. We will first create tests for login and logout actions, and then we will create these actions. After that, we will implement user authentication for the Inventory module. For this, we will create an employee registration. Finally, we will implement a user identity check based on events.

In this chapter, we’ll be covering the following topics:

+   * Creating login and logout actions for the Inventory module
+   * Implementing user authentication for the Inventory module
+   * Creating employee registration
+   * Verifying the identity of users based on events

At the end of this chapter, you will be able to control who can access restricted areas of a web application.

Let us first start creating a login page in the Inventory module and the related controller action.

# ## Technical requirements

All the code related to this chapter can be found at https://github.com/PacktPublishing/PHP-Web-Development-with-Laminas/tree/main/chapter09/whatstore.