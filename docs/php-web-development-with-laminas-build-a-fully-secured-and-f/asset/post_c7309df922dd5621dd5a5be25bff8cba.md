
Because of this, we have to modify the Employee::encrypt method for it to generate the same output for the same input. Let us modify the Employee::encrypt method with the following content:

```
public static function encrypt(String $text)
{
    $text = strrev(hash('sha256', $text));
    return hash('md5', $text);
}
```

To regenerate the password hash, you need to edit the employee Steven Grant at http://localhost:8000/inventory/employee. Now, type the same password, moonknight. In sequence, go back to the login page at http://localhost:8000/inventory, fill in the login form, and click on the login button. This time, the result will be the message shown in Figure 9.10:

![](img/e6cf5550804ccae9d1859270da8688f5_248_0.png)

### Figure 9.10 – The message for a successful employee login

After you click on the OK button, you will see an error page, as shown in Figure 9.11:

![](img/e6cf5550804ccae9d1859270da8688f5_248_1.png)

### Figure 9.11 – The error page showed after login

This was expected because we didn't create the MenuController class and the view file for the index action. To solve this, we have to follow these steps:

- 1. Create a MenuController class in the module/Inventory/src/Controller folder:

### MenuController.php

```php
<?php
namespace Inventory\Controller;
use Laminas\Mvc\Controller\AbstractActionController;
use Laminas\View\Model\ViewModel;
class MenuController extends AbstractActionController
{
    public function indexAction()
    {
        return new ViewModel();
    }
}
```

- 2. Add the following entry to the controllers => aliases key in the modules.config.php file:

```
'menu' => Controller\MenuController::class
```

- 3. Add the following entry to the controllers => factories key in the modules.config.php file:

```
Controller\MenuController::class => InvokableFactory::class
```

- 4. Change the value of the router => routes => inventory => options => defaults => controller key with index like so:

```
'controller' => 'index'
```

- 5. Create an index.phtml file in the module/Inventory/view/inventory/menu folder (you need to create the menu folder):

```
<h1>Inventory Module</h1>

<a href="<?= $this->url('inventory', ['action' =>
            'logout']) ?>">Logout</a>
```

- 6. After these changes, reload the page at `http://localhost:8000/inventory/menu`, and you will see the page shown in **Figure 9.12**:

![](img/e6cf5550804ccae9d1859270da8688f5_250_0.png)

### Figure 9.12 – The menu page for Inventory Module

Now, if you click on the **Logout** option, you will go back to the login form page. Great! It seems that it is working. However, if you try to access any other page of the **Inventory** module now, such as `http://localhost:8000/inventory/menu`, you will notice that you don't need to be authenticated to access any page. Then, the login page is useless. This is because we haven't verified the user identity yet. But don't worry! We will do that in the next section.

## Verifying user identity based on events

To verify the identity of a user and know whether they are logged in, we will use a `listener` class. As we have seen in *Chapter 7, Request Control and Data View*, Laminas uses event-driven programming to process the steps of a request life cycle. There are some defined events, and we can create listeners to act when these events are triggered.

As we don't need to store data for this verification, we will use a `static` method (a class method). Therefore, we don't need to instantiate a class. We will create a new folder named `Listener` in `modules/Inventory/src`. Inside this new folder, we will create a class named `InventoryAuthenticationListener`. This class will have a method named `verifyIdentity`, as shown in the following source code in sequence:

### InventoryAuthenticationListener.php

```php
<?php
namespace Inventory\Listener;

use Laminas\Mvc\MvcEvent;

use Laminas\Authentication\AuthenticationService;

use Laminas\Http\PhpEnvironment\Request;

class InventoryAuthenticationListener
{
    public static function verifyIdentity(MvcEvent $event)
    {
        $routeName = $event->getRouteMatch()->getMatchedRouteName();
        if (! $routeName == 'inventory') {
            return;
        }

        $authenticationService = new AuthenticationService();
        if (! $authenticationService->hasIdentity()) {
            $params = $event->getRouteMatch()->getParams();
            $controller = $params['controller'];
            $action = $params['action'];
            if ($controller == 'index' && ($action == 'index' || $action == 'login')) {
                return;
            }

            $event->getRouteMatch()->setParam('controller', 'index');
            $event->getRouteMatch()->setParam('action', 'index');
        }
    }
}
```

- The verifyIdentity method receives a MvcEvent object. The MvcEvent class is a specialization of the Laminas\EventManager\Event class for Laminas MVC implementation. It means that MvcEvent class has the generic implementation of the Event class for any kind of event and specific implementation for MVC events. An MvcEvent object has access to the Application instance and every object configured in the modules.config.php file.

- We use the `getRouteMatch` method of `MvcEvent` to recover a `RouteMatch` object. This object stores data about the matched route. The `RouteMatch->getMatchedRouteName` method returns the name of the route that matches the requested URL.

- We use the route name to know whether the requested module is `Inventory`. As `InventoryAuthenticationListener` is only for the `Inventory` module, we abort the `verifyIdentity` method if the request is from another module.

- If the requested module is `Inventory`, we instantiate the `AuthenticationService` class and invoke the `hasIdentity` method. This method returns `true` only if a user has been authenticated.

- If a user has not been authenticated, we recover the controller and action name from the matched route through the `getParams` method of the `RouteMatch` instance.

- The employee – that is, the inventory user – can access the `IndexController->indexAction` and `IndexController->loginAction` methods without authentication, and then these two methods are ignored by the authentication listener class.

- For any other controller and action of the `Inventory` module, if the user is not authenticated, we redirect them to the login page.

Note that we are intercepting the application request before a controller is instantiated. We don’t need to make an HTTP request to redirect the user to the login page. The target change is made along the same HTTP request.

However, who will invoke `InventoryAuthenticationListener`? A listener class is invoked by an event. We associate listeners and events with the `Laminas\EventManager\EventManager` class. The `Laminas\Mvc\Application` object, created in the `index.php` file, has an instance of `Laminas\EventManager\EventManager`. We will use this instance to add `InventoryAuthenticationListener` as a listener for the `EVENT_ROUTE` event. The `EVENT_ROUTE` event is dispatched after the route is matched, so we have all the necessary data about the desired resources of the user and the initial target of the request.

The last line of the `index.php` file in the `public` folder is this:

```
Application::init($appConfig)->run();
```

Let us modify this preceding line, dividing the method invocations with these two steps:

```
$app = Application::init($appConfig);
$app->run();
```

We are doing the same thing in these lines that we did in the preceding line but in different lines. The instance of `Laminas\Mvc\Application` is stored now in the `$app` variable. Let us introduce a new line between them:

```php
$app = Application::init($appConfig);
$app->getEventManager()->attach(MvcEvent::EVENT_ROUTE,
    [InventoryAuthenticationListener::class, 'verifyIdentity']);
$app->run();
```

What are we doing here? The attach method of EventManager associates a listener to an event. In other words, when the specified events are dispatched, the listener is notified. A listener can be a static method, an object method, or a function. For static methods, we need to pass an array with two elements – the class name and the method name. For the object method, we need to pass the instance as the first element of the array. For a function, only the function name is needed as the second argument of the attach method.

> **Remember, remember the namespaces!**
>
> Remember that InventoryAuthenticationListener is defined for the `Inventory\Listener` namespace, so your index.php file must have `use Inventory\Listener\InventoryAuthenticationListener;` at the top. The lack of namespaces is a common source of errors.

Let us modify the menu view to show the user identity. Change the content of the MenuController->indexAction method of the Inventory module with the following content:

```php
public function indexAction()
{
    $auth = new AuthenticationService();
    return new ViewModel(['user' => $auth->getIdentity()]);
}
```

The getIdentity method of the AuthenticationService class returns a matched identity in the authentication. In sequence, change the content of the index.phtml file in the module/Inventory/view/inventory/menu folder with the following content:

```html
<h1>Inventory Module</h1>

<p>Hi, <?=$this->user?></p>
<a href="<?=$this->url('inventory', ['action' => 'logout'])?>">Logout</a>
```
