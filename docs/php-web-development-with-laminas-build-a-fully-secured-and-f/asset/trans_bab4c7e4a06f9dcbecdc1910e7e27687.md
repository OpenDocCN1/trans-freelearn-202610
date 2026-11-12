

```php
$input->setFilterChain($filterChain);
$inputFilter->add($input);
return $inputFilter;
}
```

InputFilter 类是 Input 实例的集合。Input 类允许你为输入属性链式调用过滤器和验证器。对于 Discount 类的每个属性，我们都创建了一个 Input 实例。

要将过滤器连接到输入属性，我们使用 FilterChain 类。FilterChain 连接一组过滤器，并允许你将所有这些过滤器应用到一个值上。按顺序，这些过滤器被附加到 FilterChain。

你可以在 https://docs.laminas.dev/laminas-filter/standard-filters 找到关于 Laminas 标准过滤器类的参考。对于 Discount 类，我们使用了以下过滤器类：

- ToInt 类用于确保折扣代码是整数。
- StringToUpper 类用于确保折扣名称为大写字母。此外，Alnum 确保只允许字母数字值。事实上，Alnum 构造函数中的参数 true 也允许空格。
- AllowList 类用于将运算符的可能值限制为三种可能性。
- ToFloat 类用于确保 factor 是浮点数。

> **重要说明：过滤器类**
>
> 除了标准过滤器之外，还有更多可用的过滤器。你可以在 https://docs.laminas.dev/laminas-filter 找到可用 Laminas 过滤器的完整列表。此外，你可以通过实现 Laminas\Filter\FilterInterface 来创建自己的过滤器类。我们将在第 13 章「技巧与窍门」中学习如何创建自定义过滤器。

既然我们已经定义了生成 InputFilter 实例的方法，我们将使用该实例来转换 Discount 模型的属性值。这是通过 Discount->toArray 方法完成的，如下所示：

```php
public function toArray()
{
    $inputFilter = $this->getInputFilter();
    $inputFilter->setData(get_object_vars($this));
    return $inputFilter->getValues();
}
```

这一更改足以确保应用过滤器。现在你可以插入或更新折扣，你会看到名称将以大写字母列出，如图 8.2 所示：

![](img/e6cf5550804ccae9d1859270da8688f5_226_0.png)

图 8.2: StringToUpper 过滤器的效果

在下一节中，我们将学习如何使用 laminas-validator 验证输入值。

## 验证输入数据

过滤器转换数据，但这不足以确保数据适合我们的系统。过滤器只是执行一个操作。要了解数据是否具有适当的值，我们需要一个能够回答数据是否符合业务规则的问题的组件。

Laminas 为此提供了一个组件：laminas-validator。正如我们之前所说，laminas-validator 是 laminas-inputfilter 的依赖项。因此，如果你已经安装了 laminas-inputfilter，就不需要安装 laminas-validator。

我们说过，验证取决于业务规则。想象一下，你的客户在被询问需求时告诉你，折扣名称必须至少有三个字符。我们有一个过滤器将字母字符更改为大写字母，以及另一个过滤器删除非字母、非数字或非空格的字符。但是，这些过滤器不能确保最终名称至少有三个字符。

我们需要为名称字段创建一个验证规则。让我们修改 Discount->getInputFilter 方法中处理名称字段的部分：

```php
$input = new Input('name');
$filterChain = new FilterChain();
$filterChain->attach(new StringToUpper())
    ->attach(new Alnum(true));
$input->setFilterChain($filterChain);
```

```php
$validatorChain = new ValidatorChain();
$validatorChain->attach(new StringLength(
    ['min' => 3]
));
$input->setValidatorChain($validatorChain);
$inputFilter->add($input);
```

请注意，我们创建了一个 `ValidatorChain` 实例。`ValidatorChain` 连接一组验证器，并允许你将所有这些验证器应用到一个值上。按顺序，这些验证器被附加到 `ValidatorChain`。

你可以在 https://docs.laminas.dev/laminas-validator/standard-validators 找到关于 Laminas 标准验证器类的参考。对于 `Discount` 类，我们使用了 `StringLength` 验证器。此验证器确保文本具有最小和/或最大长度。

我们将在 `DiscountAPIController` 的插入和更新方法中添加验证。`DiscountAPIController->create` 必须如下所示：

```php
public function create($data)
{
    $discount = new Discount();
    $inputFilter = $discount->getInputFilter();
    $inputFilter->setData($data);
    if (!$inputFilter->isValid()) {
        return new JsonModel(['inserted' => 'invalid']);
    }

    $discount->exchangeArray($data);
    $inserted = $this->discountTable->save($discount);
    return new JsonModel(['inserted' => $inserted]);
}
```

请注意，我们在尝试保存数据之前应用了折扣名称的验证规则。数据库也可能有业务规则，但应用程序中的验证可以避免数据库错误以及应用程序与数据库之间不必要的通信。我们使用 `Discount->getInputFilter` 方法获取 `InputFilter` 实例，这很合理。`InputFilter->setData` 方法分配要验证的值，而 `InputFilter->isValid` 方法根据验证器链回答所有值是否有效。

我们还将修改 DiscountAPIController->update：

```php
public function update($id, $data)
{
    $discount = $this->discountTable->getByField
        ('code', $id);
    $inputFilter = $discount->getInputFilter();
    $inputFilter->setData($data);
    if (!$inputFilter->isValid()) {
        return new JsonModel(['inserted' => 'invalid']);
    }

    $discount->exchangeArray($data);
    $updated = $this->discountTable->save($discount);
    return new JsonModel(['updated' => $updated]);
}
```

你可以观察到，如果数据无效，操作方法将返回一个值为字符串 invalid 的 inserted 键。我们将使用此值向用户显示适当的消息并避免不必要的重定向。在以下代码片段中，我们展示了折扣的 edit.js 文件的内容。首先，我们将声明一个 save 函数。该函数从变量声明开始，这些变量接收来自页面 edit.phtml 的值：

```javascript
var save = function() {
    var api = $("#api").attr("url");
    var target = $("#target").attr("url");
    var formData = $("#discount").serialize();
    var code = parseInt($("input[name='code']").val());
}
```

code 变量的值指示操作是插入还是更新。对于插入，我们通过 HTTP POST 请求发送表单数据：

```javascript
if (code == 0) {
    $.post(api, formData, function(data) {
        if (data.inserted == 'invalid') {
            alert('Invalid data!');
            return;
        }
        alert("discount has been inserted");
        window.location.href = target;
    }).fail(function() {
        alert('error');
    });
}
```

对于更新，我们通过 HTTP PUT 请求发送表单数据：

```javascript
} else {
    api = $("#api").attr("url") + '/' + $(this).
        attr('code');
    $.ajax({url: api, type: 'PUT', data: formData}).
    done(function(data) {
        if (data.inserted == 'invalid') {
            alert('Invalid data!');
            return;
        }
        alert("discount has been updated");
        window.location.href = target;
    }).fail(function() {
        alert('error');
    });
}
```

最后，save 函数与按钮点击事件相关联：

```javascript
$(document).ready(function() {
    $("#save").click(save);
});
```

此修改侧重于测试异步请求内容。如果返回时，inserted 键的值为 invalid，将显示一条消息，说明数据无效。因此，如果你尝试添加一个名称短于三个字符的折扣，你将看到类似图 8.3 的消息：

