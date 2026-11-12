Does it mean we need to authenticate beforehand to test? Not really. We want to test units, not complete processes. It is enough, for now, to disable the authentication checking for testing context. This can be done by adding a conditional instruction to the Module : onBootstrap method in the Inventory module. We verify whether the PHPUNIT_COMPOSER_INSTALL constant was defined. This constant is defined by PHPUnit, installed as a Composer dependency, which it is in our case.

```
Public function onBootstrap($e)
{
    if (defined('PHPUNIT_COMPOSER_INSTALL')) return;
    $application = $e->getApplication();
    $application->getEventManager()->attach(MvcEvent : :
    EVENT_ROUTE,
    [InventoryAuthenticationListener : : class,'verifyIdentity']) ;
}
```

Finally, you can run the tests successfully!

# Summary

In this chapter, we have learned how to implement a checking process to authenticate users with event-oriented programming.

First, we created a login page for the Inventory module, handled by the IndexController class. In sequence, we installed the laminas-authenticator component and used it to authenticate the Inventory module users – the employees. Then, we learned how to implement the employee registration pages by remembering what we learned in Chapter 7, Request Control and Data View, and Chapter 8, Creating Forms and Implementing Filters and Validators. Finally, we learned how to verify authenticated users with laminas-eventmanager.

In the next chapter, we will learn how to implement authorization for users using event-driven programming.