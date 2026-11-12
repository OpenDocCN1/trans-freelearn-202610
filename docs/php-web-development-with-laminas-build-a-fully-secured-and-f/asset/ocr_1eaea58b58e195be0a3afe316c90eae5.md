```
```php
$this->assertControllerClass
('IndexController');
$this->assertMatchedRouteName('store');
}
```
```

+   7. Then, according to what we already have done, remove the `testIndexActionViewModelTemplateRenderedWithinLayout` method from `StoreTest\Controller\IndexControllerTest`. Don't forget to remove the `testInvalidRouteDoesNotCrash` method too.

As we should have learned, we need to create this route in the `module.config.php` file of the Store module. The `routes` key in this file must be as follows:

```
```php
'routes' => [
    'store' => [
        'type' => Segment::class,
        'options' => [
            'route' => '/store[:/controller[/:action[/:key]]]',
            'defaults' => [
                'controller' => Controller\\IndexController::class,
                'action' => 'index',
            ],
        ],
    ],
],
```
```

Great – for a while, our tests are running successfully. From now on, we will implement the controller and view layers for products using tests. Our controller layer for products will have two controller classes, one for receiving requests from an HTML default page, part of the whatstore application, and another class for receiving requests from any client. This architecture allows other clients, such as mobile applications, to consume services from whatstore. Figure 7.3 depicts that service-oriented architecture: