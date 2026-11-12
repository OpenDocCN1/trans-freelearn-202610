6. After these changes, reload the page at `http://localhost:8000/inventory/menu`, and you will see the page shown in **Figure 9.12**:

![](bbox=[0.31, 0.15, 0.685, 0.325])

Figure 9.12 – The menu page for Inventory Module

Now, if you click on the **Logout** option, you will go back to the login form page. Great! It seems that it is working. However, if you try to access any other page of the **Inventory** module now, such as `http://localhost:8000/inventory/menu`, you will notice that you don't need to be authenticated to access any page. Then, the login page is useless. This is because we haven't verified the user identity yet. But don't worry! We will do that in the next section.

# # Verifying user identity based on events

To verify the identity of a user and know whether they are logged in, we will use a `listener` class. As we have seen in *Chapter 7, Request Control and Data View*, Laminas uses event-driven programming to process the steps of a request life cycle. There are some defined events, and we can create listeners to act when these events are triggered.

As we don't need to store data for this verification, we will use a `static` method (a class method). Therefore, we don't need to instantiate a class. We will create a new folder named `Listener` in `modules/Inventory/src`. Inside this new folder, we will create a class named `InventoryAuthenticationListener`. This class will have a method named `verifyIdentity`, as shown in the following source code in sequence:

# **InventoryAuthenticationListener.php**

```
```php
<?php
namespace Inventory\\Listener;

use Laminas\\Mvc\\MvcEvent;

use Laminas\\Authentication\\AuthenticationService;
```
```