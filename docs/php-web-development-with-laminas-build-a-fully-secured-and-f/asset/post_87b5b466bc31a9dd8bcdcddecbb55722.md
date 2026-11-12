
You can see that we have two pairs of classes: an API controller and its factory and a web interface controller and its factory. It is exactly the same structure that we used for the product controller layer implementation. These controller classes are tested by the following classes in the module/Inventory/test/Controller folder:

- DiscountAPIControllerTest
- DiscountControllerTest

We also have a view layer implementation for discounts that is very similar to what we made for products. So, we are assuming that the following view scripts are in the module/Inventory/view/inventory/discount folder:

- edit.js
- edit.phtml
- index.js
- index.phtml

Here, we have two pairs of files that generate two web pages: the inserting/updating page and the listing page. The .phtml files have a combination of HTML and PHP code and the .js files have exclusively JavaScript code.

> Important note: .phtml content
>
> Although you can use PHP code in a .phtml file, it is a good practice to reduce this use. Model classes are responsible for business rules processing, so view script files must only show values sent from the models by controllers.

Right, imagine we have a discount registration working, as well as product registration. The edit.phtml file that generates an HTML form for inserting/updating has this content:

### edit.phtml

```php
<?php
$this->headScript()->appendScript(file_get_contents
        (__DIR__ . '/edit.js'));
?>
<h1>Discount <?= (empty($this->discount->code) ?
        'insert' : 'update') ?>!</h1>
<form id="discount">
Name: <input type="text" name="name" autofocus="autofocus" value="<?= $this->discount->name ?>"><br/>
Operator: <select name="operator">
<option value="" <?= ($discount->operator == '-' ?
'selected' : '') ?> ></option>
<option value="*" <?= ($discount->operator == '*' ?
'selected' : '') ?>>*</option>
<option value="/" <?= ($discount->operator == '/' ?
'selected' : '') ?> /></option>
</select><br/>
Factor: <input type="number" name="factor" value="<?= $this->discount->factor ?>"><br/>
<input type="hidden" name="code" value="<?= $this->discount->code ?>">
</form>
<button id="save">save</button>
<p>
<span id="api" url="<?= $this->url
('inventoryapi', ['controller' => 'discountapi'])
?>"></span>
<span id="target" url="<?= $this->url ('inventory',
['controller' => 'discount']) ?>"></span>
<a href="<?= $this->url ('inventory', ['controller' =>
'discount']) ?>">Go back</a>
</p>
```

The source code of the edit.phtml file combined with the layout.phtml file (from the Application module) produces the web page shown in Figure 8.1. Observe that the content of edit.phtml is a simple HTML form whose elements have values dynamically defined by PHP code.

## Laminas MVC Skeleton Home

## Discount insert

Name:
Operator: -
Factor: 0
save
Go back

© 2022 Laminas Project a Series of LF Projects, LLC.

Figure 8.1: Form for inserting discount

We will change the way this HTML form is generated. Of course, you could argue “Why do we need to define a form directly with HTML? There are JavaScript libraries that produce rich web interfaces. We could use them!” That's right, you could use any JavaScript library or framework for your user interface. Laminas does not require you to use HTML directly for creating forms or to use a JavaScript component. You are free to use what you wish for a frontend application. Remember we have APIs in our controller layer.

But it is good to know how to generate HTML forms programmatically from the application backend if you want to keep direct control over your web user interface. In addition, you will always have a default web interface to operate the system. More importantly, the Laminas component for generating forms is not only useful for generating a user interface but also for data validation. We will see this shortly.

We will delegate the generation of the discount form to `laminas-form`. To do that, we need to create a class to define the form.

## Creating a class to define an HTML form

Follow these steps to create a class defining an HTML form:

- First, in the module/Inventory/src folder, create a folder named Form.
- Next, inside the Form folder, create a DiscountForm.php file. This file must contain a DiscountForm class:

### DiscountForm.php

```php
<?php
namespace Inventory\Form;
use Laminas\Form\Form;
use Laminas\Form\Element\Text;
use Laminas\Form\Element\Select;
use Laminas\Form\Element\Number;
use Laminas\Form\Element\Hidden;
class DiscountForm extends Form {
    public function __construct($name = 'discount') {
        parent::__construct($name);
        $text = new Text('name');
        $text->setLabel('Name:');
        $text->setAttribute('autofocus', 'autofocus');
        $this->add($text);
        $select = new Select('operator');
        $select->setLabel('Operator:');
        $options = [
            '-=' => '-=',
            '*=' => '*=',
            '/=' => '/=',
        ];
        $select->setValueOptions($options);
        $this->add($select);
        $number = new Number('factor');
        $number->setLabel('Factor:');
        $this->add($number);
        $hidden = new Hidden('code');
        $this->add($hidden);
    }
}
```

We can observe the following from the DiscountForm class:

- DiscountForm inherits implementation from the Laminas\Form\Form class.
- The superclass constructor receives the value for the name attribute of the HTML form as an argument.
- Inside the inheritor Laminas\Form\Form class constructor, we define the form elements.
- Each HTML form element has a class with an appropriate name. For example, for HTML INPUT elements with type equal to “text,” there is the Laminas\Form\Element\Text class.
- The Laminas\Form\Element class family groups HTML form elements.
- The Laminas\Form\Element constructor receives a value as the first argument, which will be used both for the id attribute and the name attribute.
- Both the Laminas\Form\Form and Laminas\Form\Element class constructors have a second argument, but it is optional.
- The setLabel method allows you to define a label for an HTML form element.
- The setAttribute method allows you to define the value of any HTML element attribute. For example, we have defined a value for the autofocus attribute of the name input field.
- Laminas\Form\Element instances need to be associated to the Laminas\Form\Form instance by the Laminas\Form\Form->add method.

Right, but how do we use the DiscountForm class? Well, we need to create a DiscountForm instance in the web interface controller and pass it on to the appropriate view script. So, we will change the DiscountController->editAction method for the following:

```php
public function editAction()
{
    $key = $this->params('key');
    $discount = $this->discountTable->getByField
    ('code',$key);
    $form = new DiscountForm();
    $form->bind($discount);
    return new ViewModel([
        'discount' => $discount,
        'form' => $form
    ] );
}
```

Observe that we have created an instance of `DiscountForm` and we have invoked the `DiscountForm->bind` method. This method is inherited from `Laminas\Form\Form`. The `bind` method assigns the attribute values of a model object to the related fields of forms. We talk to `DiscountForm` to get the attribute values of the `Discount` model instance and use them to fill the HTML form elements.
But how does the `bind` method extract the values from the model object? Well, the `bind` method invokes the `getArrayCopy` method from the model. Wait! The `Discount` class doesn't have this method. We need to implement it! In fact, it is better to implement the `getArrayCopy` method for all models, so every one of them can work with `Laminas\Form\Form`.

Because of this, we will add a `getArrayCopy` method to the `AbstractModel` class. This class was created in _Chapter 4, From Object-Relational Mapping to MVC Containers_, and it belongs to the Generic module. The implementation of the `getArrayCopy` method is simple:

```php
public function getArrayCopy()
{
    return get_object_vars($this);
}
```

Right. Now we can modify the `edit.phtml` file for discounts. The new implementation will be the following:

### edit.phtml

```php
<?php
$this->headScript()->appendScript(file_get_contents
    (__DIR__ . '/edit.js'));
?>
<h1>Discount <?= (empty($this->discount->code) ? 'insert' : 'update') ?></h1>
<?php
$form = $this->form;
echo $this->form()->openTag($form);
echo $this->formLabel($form->get('name'));
echo $this->formText($form->get('name')) . '<br/>';
echo $this->formLabel($form->get('operator'));
echo $this->formSelect($form->get('operator')) . '<br/>';
echo $this->formLabel($form->get('factor'));
```
