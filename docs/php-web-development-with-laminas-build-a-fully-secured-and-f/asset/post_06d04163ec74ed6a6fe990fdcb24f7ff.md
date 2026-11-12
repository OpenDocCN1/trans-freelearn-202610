
## Request Control and Data View

This chapter will demonstrate how to use the Laminas implementations for the controller and view layers in an MVC application. We will start with a brief review of the relationship between PHP and **Hypertext Transfer Protocol (HTTP)**. We will look at how Laminas deals with HTTP requests and responses. After that, we will implement the controllers and views of the `Inventory` module, that is, we will learn how a module works in the Laminas MVC. Finally, we will learn how to create a page controller and page templates, along with learning about how a page controller works.

In this chapter, we'll be covering the following topics:

- Understanding the relationship between HTTP and PHP
- The request life cycle in the Laminas MVC
- Implementing CRUD with a controller and view pages

By the end of this chapter, you will be able to implement a registration using a Laminas module with an MVC pattern and you will understand how the `laminas-mvc` component handles HTTP requests. In addition, you will know how the Laminas controller page classes work and you will be able to manage HTML pages with Laminas modules.

### Technical requirements

All the code related to this chapter can be found at `https://github.com/PacktPublishing/PHP-Web-Development-with-Laminas/tree/main/chapter07/whatstore`.

### Understanding the relationship between HTTP and PHP

The essence of HTTP is this: a computer called the client sends a text message (or a request) to another computer called the server. The server replies with a text message (or a response). HTTP has been defined by RFCs 7230 to 7237 since 2014. According to RFC 7230, “client” and “server” are roles, so the same computer can be a client and a server. In a distributed system, based on microservices, for instance, each microservice can act as a client for a microservice as well as a server for another one.

Some programming languages were adapted to build web applications. PHP, however, is a language that was originally created to work with HTTP. Rasmus Lerdorf developed PHP because he was not satisfied with the tools available for creating dynamic pages, or more precisely, HTML pages integrated with databases. PHP has evolved in parallel to the protocols used for web application development. It is very fast to make dynamic pages available with PHP. The challenge is not to complicate a PHP application and keep the application under control as it grows.

Laminas has a component that encapsulates HTTP elements as objects. This is the `laminas-http` component. This component is used by `laminas-mvc`. The `laminas-http` component implements interfaces defined by `laminas-stdlib`. The `laminas-stdlib` component represents the highest level of reuse for the Laminas framework, with interfaces and abstract classes used by most of the Laminas components.

We will see how `laminas-mvc` uses classes from `laminas-http`. In the following section, we will learn about the request life cycle in a Laminas MVC application.

### The request life cycle in the Laminas MVC

At this moment, you can quickly launch the `whatstore` application and see the home page with the embedded web server for PHP. From the `whatstore` directory, you can type the following command:

```
php -S localhost:8000 -t public/
```

When you open `localhost:8000` in your browser, you will see the same welcome page that you will have seen in Chapter 2, Setting Up the Environment for Our E-Commerce Application. Of course, we haven't changed any view yet.

In Chapter 5, Creating the Virtual Store Project, we learned a little about the `public` directory. We learned that the structure of an MVC Laminas application is like that of a medieval castle, where the only way to enter is over the drawbridge. The drawbridge here is the `public` directory. All HTTP requests are passed to the `index.php` file inside the `public` directory. So, what does `index.php` do? It does this:

- Imports the `autoload.php` Composer file.
- Checks whether the Laminas Application class exists.
- Retrieves the configuration. If there is a development configuration, this configuration overrides the production configuration.
- Instantiates the `Application` class using the `Application::init` method. This instance receives objects from `laminas-http` via dependency injection.
- Executes the `Application->run` method from the created instance.

When index.php executes Application->run, an inversion of control happens. From this point onward, Laminas assumes control and will call your specific code, or, in other words, your business code.

> Note: inversion of control
> You can read more about the inversion of control pattern in the following article by Martin Fowler: https://martinfowler.com/bliki/InversionOfControl.html.

Figure 7.1 gives a general view of the life cycle of a request in a Laminas MVC application:

![](img/e6cf5550804ccae9d1859270da8688f5_177_0.png)

Figure 7.1 – HTTP request life cycle in a Laminas MVC application

Laminas uses event-driven programming for processing the steps of the request life cycle: bootstrapping, routing, dispatching the controller, and rendering the view. We can divide this life cycle into two main moments:

- When the `Application::init` method is executed, `ServiceManager` gets an instance of `ModuleManager`, which loads the modules registered in the `modules.config.php` file. The `Application::init` method also triggers the `EVENT_BOOTSTRAP` event using the `bootstrap` method. The `EventManager` class notifies the listeners of this event for executing the bootstrap tasks.

- When the `Application->run` method is executed, `Application` triggers the `EVENT_DISPATCH` event. The `EventManager` class notifies the listeners of this event and one of them instantiates a controller class. This controller class, in turn, renders a view for the HTTP response. This view can be a JSON object, an HTML page, or potentially any other format that can be consumed by another application.

Wait a moment. Which controller will be called, exactly? It depends on the route. As we already said in *Chapter 4, From Object-Relational Mapping to MVC Controllers*, Laminas defines routes in its modules. Routes are associations between URL patterns and controller classes. The `Application->run` method also triggers an event called `EVENT_ROUTE`, and `EventManager` notifies certain listeners related to `ModuleManager` (`ModuleRouteListener` and `RouteListener`). These listeners define the controller to be called from the route that matches the URL.

We will understand this life cycle better in the next section, where we will implement a controller and view pages for product registration.

### Implementing CRUD with a controller and view pages

We will create a controller in the `Inventory` module for managing products. Following the good practice of TDD, we will create this controller from tests.

In this section, we will first test the current test cases. Then, we will create the tests for inserting, recovering, updating, and deleting products.

### Testing our current test cases

The first step is to check the current state of our test cases by following these steps:

- 1. Select the `whatstore` project and right-click on **Run As/PHPUnit Test**. You will see the output of PHPUnit in the **PHPUnit** tab, as *Figure 7.2* shows:

![](img/e6cf5550804ccae9d1859270da8688f5_179_0.png)

Figure 7.2 – PHPUnit output

As you can see, the `IndexControllerTest` class from the `Inventory` module has failed. It failed because this class is a copy of `IndexControllerTest` from the `Application` module and the routes and pages are different. Let us fix this class.

- 2. First, change the `testIndexActionCanBeAccessed` method for `Inventory\\Controller\\IndexControllerTest` to the following code:

```
public function testIndexActionCanBeAccessed(): void
{
    $this->dispatch('/inventory', 'GET');
    $this->assertResponseStatusCode(200);
    $this->assertModuleName('inventory');
    $this->assertControllerName(IndexController::class); // as specified in router's controller name alias
    $this->assertControllerClass('IndexController');
    $this->assertMatchedRouteName('inventory');
}
```

- 3. Then, remove the `testIndexActionViewModelTemplateRenderedWithinLayout` method from `InventoryTest\\Controller\\IndexControllerTest`. This method only works for *whatstore* home page.

- 4. Remove the `testInvalidRouteDoesNotCrash` method too. This method is already defined for the `Application` module.

If you execute **Run As/PHPUnit Test** again, `IndexControllerTest` fails again. Now, the reason is that the `inventory` route does not exist yet. We need to create this route in the `module.config.php` file of the `Inventory` module.
