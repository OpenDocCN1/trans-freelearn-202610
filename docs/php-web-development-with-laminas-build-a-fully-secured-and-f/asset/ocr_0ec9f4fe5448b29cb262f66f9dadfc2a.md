```
$this->add($hidden);
}
}
```

We can observe the following from the DiscountForm class:

+   • DiscountForm inherits implementation from the Laminas\Form\Form class.
+   • The superclass constructor receives the value for the name attribute of the HTML form as an argument.
+   • Inside the inheritor Laminas\Form\Form class constructor, we define the form elements.
+   • Each HTML form element has a class with an appropriate name. For example, for HTML INPUT elements with type equal to “text,” there is the Laminas\Form\Element\Text class.
+   • The Laminas\Form\Element class family groups HTML form elements.
+   • The Laminas\Form\Element constructor receives a value as the first argument, which will be used both for the id attribute and the name attribute.
+   • Both the Laminas\Form\Form and Laminas\Form\Element class constructors have a second argument, but it is optional.
+   • The setLabel method allows you to define a label for an HTML form element.
+   • The setAttribute method allows you to define the value of any HTML element attribute. For example, we have defined a value for the autofocus attribute of the name input field.
+   • Laminas\Form\Element instances need to be associated to the Laminas\Form\Form instance by the Laminas\Form\Form->add method.

Right, but how do we use the DiscountForm class? Well, we need to create a DiscountForm instance in the web interface controller and pass it on to the appropriate view script. So, we will change the DiscountController->editAction method for the following:

```
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
```