

```
echo $this->formNumber($form->get('factor')) . '<br/>'; echo $this->formHidden($form->get('code')); echo $this->form()->closeTag(); ?> <button id="save">save</button> <p> <span id="api" url="<?=$this->url('inventoryapi', ['controller' => 'discountapi']) ?>"></span> <span id="target" url="<?=$this->url('inventory', ['controller' => 'discount']) ?>"></span> <a href="<?=$this->url('inventory', ['controller' => 'discount']) ?>">Go back</a> </p>
```

请注意，我们已将 HTML 表单元素的内容替换为一系列带有 form 前缀的方法调用。我们需要从之前的源代码中了解以下内容：

- `$this->form` 属性是由 `DiscountController` 实例发送的 `DiscountForm` 实例。
- `$this->form` 方法（末尾带括号）是一个视图助手，用于呈现使用 Laminas\Form\Form 定义的 HTML 表单。
- `$this->formLabel` 方法仅呈现表单元素的标签。
- 每种元素类型都有相应的呈现助手类。例如，operator 字段是 HTML SELECT 元素，因此我们使用 `$this->formSelect` 来呈现它。
- `Laminas\Form\Form` 的 `get` 方法返回一个 `Laminas\Form\Element` 对象。参数是传递给对象构造函数的值。

要使用视图助手类，需要在 `modules.config.php` 文件中将 `Laminas\Form` 声明为模块。如果由 `modules.config.php` 返回的数组中没有值为 "`Laminas\Form`" 的元素，则在尝试呈现表单时，您会收到诸如 **在插件管理器 `Laminas\View\HelperPluginManager` 中未找到名为 ‘form’ 的插件** 之类的消息。

将 `Laminas\Form` 作为模块添加是启用表单呈现的简洁方式。还有一种更冗长的方法。我们将在这里逐一介绍，因为这将帮助您了解幕后发生了什么，即 `laminas-mvc` 为您所做的工作。

启用表单呈现的冗长方式是在 module.config.php 文件中为每个表单助手注册别名和工厂。此注册由配置数组第一级的键 view_helpers 完成，如以下源代码所示：

```php
'view_helpers' => [
    'aliases' => [
        'form' => Form::class,
        'formText' => FormText::class,
        'formSelect' => FormSelect::class,
        'formNumber' => FormNumber::class,
        'formHidden' => FormHidden::class,
        'formLabel' => FormLabel::class
    ],
    'factories' => [
        Form::class => InvokableFactory::class,
        FormText::class => InvokableFactory::class,
        FormSelect::class => InvokableFactory::class,
        FormNumber::class => InvokableFactory::class,
        FormHidden::class => InvokableFactory::class,
        FormLabel::class => InvokableFactory::class
    ]
]
```

在 aliases 子键中，我们定义了调用视图助手类所使用的名称。在 factories 子键中，我们定义了创建视图助手实例的方式。请注意，这种方法允许我们进行自定义。这通常是 Laminas 组件的一个特性。

您可能已经注意到，将 Laminas\Form 作为模块添加更好。这正是我们将在项目中做的事情。

> **重要说明：表单视图插件**
>
> 如果您未将 Laminas\Form 声明为模块，也未在 module.config.php 中注册表单视图插件，则在尝试呈现表单时，您会收到诸如 **在插件管理器 `Laminas\View\HelperPluginManager` 中未找到名为 ‘form’ 的插件** 之类的消息。

在尝试呈现表单之前，我们还需要安装 laminas-i18n 组件。该组件允许您根据应用程序配置从翻译文件中翻译文本。安装 laminas-i18n 的方式与安装 laminas-form 相同。

## i18n

i18n 是 internationalization 的缩写。简而言之，它是对多种语言的支持。I18n 软件组件有助于将内容从一种语言翻译成另一种语言。

## 重要说明：表单与翻译器之间的依赖关系

如果您未安装 `laminas-i18n` 组件并尝试呈现由 `laminas-form` 定义的表单，您将收到消息 Class 'Laminas\\I18n\\View\\Helper\\AbstractTranslatorHelper' not found。好吧，根据此消息，表单需要一个翻译器。这一要求可能看起来很奇怪，因为它假设任何表单都必须被翻译。考虑到 Laminas 组件架构等强耦合架构，表单组件与翻译器组件之间的关系应该是可选的。但是 `laminas-form` 与 `laminas-i18n` 之间强耦合的原因是，表单组件可以集成到过滤器和验证组件中，而其中一些组件依赖于 `laminas-i18n`。

## laminas-mvc-form

如果您使用 `laminas-mvc` 且不想分别安装 `laminas-form` 和 `laminas-i18n`，可以安装 `laminas-mvc-form` 元包。

目前，我们已通过呈现 `laminas-form` 成功用 HTML 替换了折扣表单的直接定义。折扣表单的呈现未发生变化，因此我们没有新界面。另一方面，我们现在可以在控制器层将模型与表单连接起来。

因此，我们已完成使用 `laminas-form` 生成添加和更新折扣表单的工作。为此，我们执行了以下操作：

- 为折扣创建了 CRUD 实现，包含 API 和 Web 界面
- 创建了一个类来定义折扣的 HTML 表单

在下一节中，我们将学习如何使用 `laminas-filter` 过滤输入值。

## 过滤输入数据

在本章中，您已使用 Composer 添加并安装了 `laminas-form` 组件，请对 `laminas-inputfilter` 组件执行相同操作：

- 1. 在 Eclipse 中打开 `composer.json` 文件。
- 2. 通过 **依赖项** 选项卡，搜索 `laminas/laminas-inputfilter` 包并将其添加。
- 3. 不要忘记保存文件并单击 **更新依赖项** 按钮。

laminas-inputfilter 组件结合了 laminas-filter 和 laminas-validator。您可以在 PHP 程序中分别使用 laminas-filter 和 laminas-validator，但对于 MVC Web 应用程序，使用 laminas-inputfilter 更加方便。

我们有一个用于插入/更新折扣的 HTML 表单。但这是处理折扣请求的方式之一。我们有一个 API，因此另一个应用程序可以向 whatstore 发出请求。我们的应用程序面向服务，因此对多个客户端开放。这很有用，但数据处理方面存在一个问题。如果用户填写 HTML 表单，JavaScript 代码可以在发送请求之前过滤表单数据。当在没有 HTML 页面的情况下发出请求时，没有 JavaScript 来过滤数据。因此，服务器必须确保输入数据的格式有效。

当然，应用程序数据的有效格式取决于应用程序要求。这里我们假设正在阅读需求文档，以实现处理数据的代码。

定义模型数据过滤规则的地方是模型类。因此，我们将根据以下源代码向 Discount 类添加 getInputFilter 方法：

```php
public function getInputFilter(): InputFilter
{
    $inputFilter = new InputFilter();
    $input = new Input('code');
    $filterChain = new FilterChain();
    $filterChain->attach(new ToInt());
    $input->setFilterChain($filterChain);
    $inputFilter->add($input);
    $input = new Input('name');
    $filterChain = new FilterChain();
    $filterChain->attach(new StringToUpper())
        ->attach(new Alnum(true));
    $input->setFilterChain($filterChain);
    $inputFilter->add($input);
    $input = new Input('operator');
    $filterChain = new FilterChain();
    $filterChain->attach(new AllowList(['list' =>
        ['-', '*', '/']]));
    $input->setFilterChain($filterChain);
    $inputFilter->add($input);
    $input = new Input('factor');
    $filterChain = new FilterChain();
    $filterChain->attach(new ToFloat());
}
```

