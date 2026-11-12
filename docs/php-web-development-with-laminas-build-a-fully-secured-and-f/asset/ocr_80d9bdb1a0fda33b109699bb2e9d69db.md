This was expected because we didn't create the MenuController class and the view file for the index action. To solve this, we have to follow these steps:

+   1. Create a MenuController class in the module/Inventory/src/Controller folder:

# MenuController.php

```
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

+   2. Add the following entry to the controllers => aliases key in the modules.config.php file:

```
'menu' => Controller\\MenuController::class
```

+   3. Add the following entry to the controllers => factories key in the modules.config.php file:

```
Controller\\MenuController::class => InvokableFactory
            :class
```

+   4. Change the value of the router => routes => inventory => options => defaults => controller key with index like so:

```
'controller' => 'index'
```

+   5. Create an index.phtml file in the module/Inventory/view/inventory/menu folder (you need to create the menu folder):

```
<h1>Inventory Module</h1>

<a href="<?= $this->url('inventory', ['action' =>
            'logout']) ?>">Logout</a>
```