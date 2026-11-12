```
use Laminas\Form\Element\Password;

class LoginForm extends Form {
    public function __construct($name = 'user') {
        parent::__construct($name);
        $text = new Text('nickname');
        $text->setLabel('NickName: ');
        $text->setAttribute('autofocus', 'autofocus');
        $this->add($text);
        $password = new Password('password');
        $password->setLabel('Password: ');
        $this->add($password);
    }
}
```

As you can see, the LoginForm class is an extension of Laminas\Form\Form and uses element classes that we already used in Chapter 8, Creating Forms and Implementing Filters and Validators. We will use LoginForm in the IndexController class of the Inventory module. Modify the IndexController->indexAction method like so:

```
public function indexAction()
{
    $form = new LoginForm();
    return new ViewModel([
        'form' => $form
    ]);
}
```

The $form object is delivered to the view layer, and it will be handled by the index.phtml file in modules/Inventory/view/inventory/index. The content of index.phtml looks like this:

# index.phtml

```
<?php
/**
 * @var Laminas\View\Renderer\PhpRenderer $this
 */
$this->headScript()->appendScript(file_get_contents
    (__DIR__ . '/index.js'));

```