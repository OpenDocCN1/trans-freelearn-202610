
可以看到，我们有两对类：API 控制器及其工厂，以及 Web 界面控制器及其工厂。这与我们用于产品控制器层实现的结构完全相同。这些控制器类由 module/Inventory/test/Controller 文件夹中的以下类进行测试：

- DiscountAPIControllerTest
- DiscountControllerTest

我们还为折扣实现了一个视图层，它与我们为产品制作的非常相似。因此，我们假设以下视图脚本位于 module/Inventory/view/inventory/discount 文件夹中：

- edit.js
- edit.phtml
- index.js
- index.phtml

这里，我们有两对文件，生成两个网页：插入/更新页面和列表页面。.phtml 文件包含 HTML 和 PHP 代码的组合，而 .js 文件仅包含 JavaScript 代码。

> 重要提示：.phtml 内容
>
> 虽然可以在 .phtml 文件中使用 PHP 代码，但减少这种用途是良好的做法。模型类负责业务规则处理，因此视图脚本文件仅应显示由控制器从模型发送的值。

好的，假设折扣注册已正常工作，产品注册也是如此。生成用于插入/更新的 HTML 表单的 edit.phtml 文件内容如下：

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

edit.phtml 文件的源代码与 layout.phtml 文件（来自 Application 模块）组合生成了图 8.1 所示的网页。请注意，edit.phtml 的内容是一个简单的 HTML 表单，其元素值由 PHP 代码动态定义。

## Laminas MVC 骨架主页

## 折扣插入

名称：
运算符：-
因子：0
保存
返回

© 2022 Laminas 项目，LF Projects, LLC 的一个系列

图 8.1：插入折扣的表单

我们将更改生成此 HTML 表单的方式。当然，你可能会争辩：“我们为什么要直接用 HTML 定义表单？有可生成丰富 Web 界面的 JavaScript 库。我们可以使用它们！”没错，你可以为用户界面使用任何 JavaScript 库或框架。Laminas 不强制要求你直接使用 HTML 来创建表单或使用 JavaScript 组件。你可以自由为前端应用程序选择所需的工具。请记住，我们的控制器层中有 API。

但是，如果你希望直接控制 Web 用户界面，了解如何从应用程序后台以编程方式生成 HTML 表单是件好事。此外，你始终会有一个默认的 Web 界面来操作系统。更重要的是，Laminas 用于生成表单的组件不仅用于生成用户界面，还用于数据验证。我们将很快看到这一点。

我们将把折扣表单的生成委托给 `laminas-form`。为此，我们需要创建一个类来定义表单。

## 定义 HTML 表单的类

按照以下步骤创建定义 HTML 表单的类：

- 首先，在 module/Inventory/src 文件夹中，创建一个名为 Form 的文件夹。
- 接下来，在 Form 文件夹内，创建一个 DiscountForm.php 文件。该文件必须包含 DiscountForm 类：

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

从 DiscountForm 类可以观察到以下几点：

- DiscountForm 类继承自 Laminas\Form\Form 类的实现。
- 父类构造函数接收 HTML 表单的 name 属性值作为参数。
- 在继承的 Laminas\Form\Form 类构造函数内部，我们定义表单元素。
- 每个 HTML 表单元素都有一个名称合适的类。例如，对于 type 等于“text”的 HTML INPUT 元素，存在 Laminas\Form\Element\Text 类。
- Laminas\Form\Element 类系列对 HTML 表单元素进行分组。
- Laminas\Form\Element 构造函数将第一个参数作为值，该值将同时用于 id 属性和 name 属性。
- Laminas\Form\Form 和 Laminas\Form\Element 类构造函数都有第二个参数，但它是可选的。
- setLabel 方法允许为 HTML 表单元素定义标签。
- setAttribute 方法允许定义任何 HTML 元素属性的值。例如，我们为 name 输入字段的 autofocus 属性定义了一个值。
- Laminas\Form\Element 实例需要通过 Laminas\Form\Form->add 方法与 Laminas\Form\Form 实例关联。

好吧，但我们如何使用 DiscountForm 类呢？嗯，我们需要在 Web 界面控制器中创建 DiscountForm 实例，并将其传递给相应的视图脚本。因此，我们将把 DiscountController->editAction 方法更改为以下内容：

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

请注意，我们已创建 `DiscountForm` 的实例并调用了 `DiscountForm->bind` 方法。该方法继承自 `Laminas\Form\Form`。`bind` 方法将模型对象的属性值分配给表单的相关字段。我们与 `DiscountForm` 通信以获取 `Discount` 模型实例的属性值，并使用它们来填充 HTML 表单元素。

但是 `bind` 方法如何从模型对象中提取值呢？嗯，`bind` 方法会调用模型的 `getArrayCopy` 方法。等等！`Discount` 类没有这个方法。我们需要实现它！事实上，为所有模型实现 `getArrayCopy` 方法更好，这样它们都可以与 `Laminas\Form\Form` 一起工作。

出于此原因，我们将向 `AbstractModel` 类添加一个 `getArrayCopy` 方法。该类在 _第 4 章，从对象关系映射到 MVC 容器_ 中创建，属于 Generic 模块。`getArrayCopy` 方法的实现很简单：

```php
public function getArrayCopy()
{
    return get_object_vars($this);
}
```

好。现在我们可以修改折扣的 `edit.phtml` 文件了。新实现如下：

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
