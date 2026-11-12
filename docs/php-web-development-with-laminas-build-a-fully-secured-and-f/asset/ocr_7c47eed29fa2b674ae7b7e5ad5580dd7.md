# # Adding a product to the products table

We will change the constructor of the ProductAdditionContext class to instantiate a **Table Data Gateway** implementation, as follows:

```
```php
public function __construct()
{
    $this->product = new Product();
    $this->productTable = $this->getApplication() ->
        getServiceManager()->get('ProductTable');
}
```
```

As we can see, we are setting an `$this->productTable` attribute. Therefore, this attribute must be declared at the beginning of the class:

```
```php
private ?ProductTable $productTable = null;
```
```

The `$this->productTable` attribute receives an object from the get method of the Laminas\ ServiceManager\ServiceManager class. We recover the instance of ServiceManager from an instance of Laminas\Mvc\Application. This last instance is created by a private `getApplication` method that we must implement in the following way:

```
```php
private function getApplication()
{
    $appConfig = require __DIR__ . '/../../../config/
    application.config.php';
    if (file_exists(__DIR__ . '/../../../config/
    development.config.php')) {
        $appConfig = ArrayUtils::merge($appConfig,
        require __DIR__ . '/../../../config/
        development.config.php');
    }

    return Application::init($appConfig);
}
```
```

The Application instance is created by the `init` method of this class, from the application configuration.