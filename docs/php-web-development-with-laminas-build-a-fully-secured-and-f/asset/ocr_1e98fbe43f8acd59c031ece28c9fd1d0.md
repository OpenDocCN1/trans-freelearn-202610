Laminas uses event-driven programming for processing the steps of the request life cycle: bootstrapping, routing, dispatching the controller, and rendering the view. We can divide this life cycle into two main moments:

+   • When the `Application::init` method is executed, `ServiceManager` gets an instance of `ModuleManager`, which loads the modules registered in the `modules.config.php` file. The `Application::init` method also triggers the `EVENT_BOOTSTRAP` event using the `bootstrap` method. The `EventManager` class notifies the listeners of this event for executing the bootstrap tasks.

+   • When the `Application->run` method is executed, `Application` triggers the `EVENT_DISPATCH` event. The `EventManager` class notifies the listeners of this event and one of them instantiates a controller class. This controller class, in turn, renders a view for the HTTP response. This view can be a JSON object, an HTML page, or potentially any other format that can be consumed by another application.

Wait a moment. Which controller will be called, exactly? It depends on the route. As we already said in *Chapter 4, From Object-Relational Mapping to MVC Controllers*, Laminas defines routes in its modules. Routes are associations between URL patterns and controller classes. The `Application->run` method also triggers an event called `EVENT_ROUTE`, and `EventManager` notifies certain listeners related to `ModuleManager` (`ModuleRouteListener` and `RouteListener`). These listeners define the controller to be called from the route that matches the URL.

We will understand this life cycle better in the next section, where we will implement a controller and view pages for product registration.

# Implementing CRUD with a controller and view pages

We will create a controller in the `Inventory` module for managing products. Following the good practice of TDD, we will create this controller from tests.

In this section, we will first test the current test cases. Then, we will create the tests for inserting, recovering, updating, and deleting products.

# Testing our current test cases

The first step is to check the current state of our test cases by following these steps:

+   1. Select the `whatstore` project and right-click on **Run As/PHPUnit Test**. You will see the output of PHPUnit in the **PHPUnit** tab, as *Figure 7.2* shows: