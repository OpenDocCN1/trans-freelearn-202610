```
ervice();
if (!$authenticationService->hasIdentity()) {
    $action = $params['action'];
    if ($controller == 'index' && ($action ==
        'index' || $action == 'login')) {
        return;
    }
}
$event->getRouteMatch()->setParam
    ('controller', 'customer');
$event->getRouteMatch()->setParam('action',
    'index');
}
}
```

Note that this StoreAuthenticationListener class only controls the requests for the Store module, in the same way that the InventoryAuthenticationListener class only controls the requests for the Inventory module. Of course, for the StoreAuthenticationListener class to listen to the route event, we need to attach it to ServiceManager in the Module.php file of the Store module, as we did in the Inventory module. We need to add the onBootstrap method to Module class, as you can see in the next snippet:

```
public function onBootstrap($e)
{
    if (defined('PHPUNIT_COMPOSER_INSTALL')) return;
    $application = $e->getApplication();
    $application->getEventManager()->attach
    (MvcEvent::EVENT_ROUTE,[StoreAuthenticationListener
    ::class,'verifyIdentity']);
}
```

We also have a view layer implementation for customers that is very similar to what we had made for products and other models. So, we are assuming that the following view scripts are in the module/Store/view/store/customer folder:

+   * index.js
+   * index.phtml
+   * register.js
+   * register.phtml