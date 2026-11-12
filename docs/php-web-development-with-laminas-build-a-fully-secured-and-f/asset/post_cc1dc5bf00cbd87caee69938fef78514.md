
## Exchanging messages between the controller and view layers

If you click on the Flash messenger button, you will be taken to the following page:

![](img/e6cf5550804ccae9d1859270da8688f5_370_0.png)

Figure 13.14 – Message stored by the FlashMessenger plugin

The Flash messenger button calls the IndexController->flashMessengerAction method like so:

```php
public function flashMessengerAction()
{
    $this->flashMessenger()->addMessage('You called
        this action at ' . date('H:i:s'));
    $messages = $this->flashMessenger()->getMessages();
    return new ViewModel(['messages' => $messages]);
}
```

The FlashMessenger class used in the controller stores messages in the session container. The messages are stored in a message stack, which can be accessed by the getMessages method. In short, the FlashMessenger class is a message manager. The normal way to send the messages stored in an instance of FlashMessenger to the view script would be to inject the output of the getMessages method in the ViewModel constructor, as we can see in the previous code snippet. But as we can see in the following code snippet, this is not necessary:

### flash-messenger.phtml

```php
<h1>Messages</h1>
<?=$this->flashMessenger()->render() ?>
```

Observe that the view script related to the `flashMessengerAction` method does not use the `$messages` variable injected in `ViewModel`. Instead of this, the view script uses a view helper of `FlashMessenger`, which has a `render` method. The `render` method avoids iterating over a message array, outputting the content as simple text.

`FlashMessenger` is a controller plugin, and it is not a part of `laminas-mvc`. You have to install it via Composer separately.

There are two ways to use a not-embedded controller plugin. The easiest is to add the plugin as a module in the `modules.config.php` file. You must add the **Fully Qualified Class Name (FQCN):**

```php
'Laminas\\Mvc\\Plugin\\FlashMessenger'
```

The alternative and more verbose way is to configure the plugin in the `module.config.php` file of the module where it will be used. You need to create a `controller_plugins` key, with `factories` and `aliases` subkeys. The following code snippet shows how the `FlashMessenger` plugin could be configured in the `tripsandtricks` application:

```php
'controller_plugins' => [
    'factories' => [
        FlashMessenger::class => InvokableFactory::class
    ],
    'aliases' => [
        'flashMessenger' => FlashMessenger::class
    ]
],
```

While the `factories` key defines how to create the `FlashMessenger` instance, the `aliases` key defines the name that you will use to call the plugin in the controller actions.

![](img/e6cf5550804ccae9d1859270da8688f5_371_0.png)

This is the configuration for the controller plugin. To use the FlashMessenger view helper, if you didn't configure the component as a module, you need to add a view_helpers key to the modules.config.php file, as follows:

```php
'view_helpers' => [
    'invokables' => [
        'flashMessenger' => \Laminas\Mvc\Plugin\FlashMessenger\View\Helper\FlashMessenger::class
    ]
]
```

The controller plugin configuration should be new to you, but the view helper configuration is not. In fact, you already coded this configuration in Chapter 8, Creating Forms and Implementing Filters and Validators, to enable form view helpers.

Next, we will see the different ways to end controller actions.

## Finishing controller actions

In most cases, a controller action can terminate when one of the following objects is returned:

- A ViewModel instance, if it is a response for a synchronous request, which expects an HTML page.
- A JSONModel instance, if it is a response for an asynchronous request, which expects JSON data.
- A call to the `$this->redirect()->toRoute()` method. This method expects two arguments; the first is the name of the route and the second is an array with route parameters. This method causes an HTTP redirecting request (HTTP status 302).

There are other Laminas\View\Model classes: ConsoleModel, for generating output for terminals, and FeedModel, for generating output for RSS consumers. You can also extend Laminas\View\Model ModelInterface and create specific outputs.

## Summary

In this chapter, we learned some useful tips and tricks relating to Laminas in the three layers of an MVC application.

We first learned how to customize filters and validators for model layers using the Laminas interfaces. We learned about the required methods for using mapped models or models integrated with forms.

After that, we learned, or better, we remembered, how to produce JSON responses for the view layer. We also learned how to upload files using laminas-form and how to change the layout configuration.

At the end, we learned how to detect AJAX requests in controllers and how to manage messages with laminas-mvc-plugin-flashmessenger. Finally, we learned the possible ways to end a controller action.

In the next chapter, we will cover a final few things. Yes, it is a pity, but this book is coming to an end...

## Last Considerations

This chapter marks the conclusion of the book, showing the possibilities going forward with other components not used in sample applications.

In this chapter, we will get a general view of Laminas components that were not used in the sample applications in earlier chapters of the book. We will learn about a component for creating distributed and decentralized applications, as well as learning about a component for creating APIs. Lastly, we will learn where we can get help with Laminas components and how to become a part of the Laminas community and improve the Laminas framework.

In this chapter, we will cover the following topics:

- Other Laminas components
- Microservice-oriented development
- Building APIs
- Resources of community

### Other Laminas components

In this section, we will get an overview of some Laminas components that were not used in the projects of this book. You can read the documentation about all the Laminas components at `https://docs.laminas.dev/components`.

### Cache

Let's use travel as an analogy. We spend time and money traveling. Since time is money, we spend a lot of money traveling. In addition, the farther we go, the more money we spend. In other words, the cheapest place to go is where you are already. Staying in the same place is the cheapest type of travel. This is the idea of the cache: to keep data in the nearest place possible to reduce the time and cost of transport. The `laminas-cache` component allows us to cache data using several storage adapters, which enable your application to save and read data from filesystem, Memcached, Redis, or MongoDB databases, among others.

### CLI

Although we have used Laminas to develop web applications, the framework is not limited to this purpose. In fact, the PHP manual says that PHP can do anything. With Laminas, we can do anything, but with more control.

> **What can PHP do?**
> You can find a description of the possible uses of PHP at https://www.php.net/manual/en/intro-whatcando.php.

Believe it or not, you can create command-line PHP programs with Laminas. To do this, you should use the laminas-cli component.

### Crypt

In our whatstore project, we said that it is important to encrypt passwords. But you shouldn’t use the algorithms that were used in the examples in this book. It is recommended to use strong cryptography and password hashing to protect and authenticate sensitive data. The laminas-crypt component offers an easy and secure way to implement cryptography in PHP applications. This component was developed by several great applied cryptography specialists, such as Enrico Zimuel, a core member of the PHP Framework Interoperability Group.

### CSRF

There is actually no component called laminas-crsf. However, there are two classes that handle CSRF. CSRF stands for cross-site request forgery, an attack where someone tries to send malicious data through falsified forms. The laminas-form component provides Element\Csrf to add a hash element to a form with timeout configuration. To validate the hash element value, you can use the Laminas\Validator\Csrf class.

### DOM

DOM stands for document object model. This is a web standard, documented at https://www.w3.org/DOM/DOMTR. According to this standard, the HTML tags can be handled as an object hierarchy. In addition, it is possible to search for and select objects using the XPath language and a CSS selector. The laminas-dom component enables you to find elements in XML and HTML documents using these two approaches. It is very useful to automate the reading of documents in these formats in order to import data or search for data on websites through the implementation of web crawlers.
