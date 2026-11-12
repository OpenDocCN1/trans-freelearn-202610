# # Request Control and Data View

This chapter will demonstrate how to use the Laminas implementations for the controller and view layers in an MVC application. We will start with a brief review of the relationship between PHP and **Hypertext Transfer Protocol (HTTP)**. We will look at how Laminas deals with HTTP requests and responses. After that, we will implement the controllers and views of the `Inventory` module, that is, we will learn how a module works in the Laminas MVC. Finally, we will learn how to create a page controller and page templates, along with learning about how a page controller works.

In this chapter, we'll be covering the following topics:

+   * Understanding the relationship between HTTP and PHP
+   * The request life cycle in the Laminas MVC
+   * Implementing CRUD with a controller and view pages

By the end of this chapter, you will be able to implement a registration using a Laminas module with an MVC pattern and you will understand how the `laminas-mvc` component handles HTTP requests. In addition, you will know how the Laminas controller page classes work and you will be able to manage HTML pages with Laminas modules.

# ## Technical requirements

All the code related to this chapter can be found at `https://github.com/PacktPublishing/PHP-Web-Development-with-Laminas/tree/main/chapter07/whatstore`.

# ## Understanding the relationship between HTTP and PHP

The essence of HTTP is this: a computer called the client sends a text message (or a request) to another computer called the server. The server replies with a text message (or a response). HTTP has been defined by RFCs 7230 to 7237 since 2014. According to RFC 7230, “client” and “server” are roles, so the same computer can be a client and a server. In a distributed system, based on microservices, for instance, each microservice can act as a client for a microservice as well as a server for another one.