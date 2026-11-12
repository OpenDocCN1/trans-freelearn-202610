![](bbox=[0.23, 0.1, 0.76, 0.37])

Figure 9.2 – Error page for controller not found

Why are we receiving this error? Because there is no alias for `IndexController` class in the Inventory module. If you observe the part of the test that checks the controller name, you can see the following comment about the alias:

```
$this->assertControllerName('index');
// as specified in router's controller name alias
```

Let us solve this issue. In the `module.config.php` file, add a line that associates the index alias with `IndexController` class like so:

```
'controllers' => [
    'aliases' => [
        'index' => Controller\\IndexController::class,
    ],
]
```

This change will reduce the failures to only one, related to the `loginAction` method of `IndexController` class. In the next section, we will implement user authentication to solve this failure.

# Implementing the user authentication for the Inventory module

For authenticating the user, we will use the `laminas-authentication` component. Add and install the `laminas-authentication` component in the same way you did with the