```
{
    $products = $this->productTable->getAll();
    return new ViewModel(['products' => $products]);
}
public function editAction()
{
    $key = $this->params('key');
    $product = $this->productTable->
        getByField('code',$key);
    $discounts = $this->discountTable->getAll();
    return new ViewModel([
        'product' => $product,
        'discounts' => $discounts
    ]);
}
}
```

**An array in place of ViewModel**

You also can return an array in place of a ViewModel object. The controller class will inject the array into a ViewModel object.

The indexAction method recovers a set of products and delivers it to the view layer. The editAction method tries to find a product by its code and delivers the product object and a set of discounts to the view layer. As you may be able to tell, the recovery of products and discounts depends on the table mapper objects. These objects are injected by the constructor method. This injection is made by a factory class, ProductControllerFactory, whose source code looks as follows:

# ProductControllerFactory.php

```
<?php
namespace Inventory\Controller;
use Laminas\ServiceManager\Factory\FactoryInterface;
use Interop\Container\ContainerInterface;
class ProductControllerFactory implements FactoryInterface
{
    public function __invoke(ContainerInterface $container,
        $requestedName, array $options = null)
```