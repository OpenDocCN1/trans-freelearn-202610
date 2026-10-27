

## Intl 库

Intl 库可以帮助你设计和开发全球化的服务器及 Web 客户端应用。它支持所有主要区域设置和子语言对。由于屏幕上的所有信息都表现为一组字符串，我们必须使用 Intl 库提供的特殊格式化器将其他类型的对象转换为字符串格式。显示字符串的翻译版本被捆绑在单独的文本文件中。Intl 库包含一个同名的类，用于处理消息。Intl 库在各个国际化领域中考虑数字、日期和消息。每个领域都特意单独初始化，以减小应用程序体积并避免加载不必要的资源。数字和日期的国际化通过格式化器实现。消息通过特殊函数进行国际化，并可导出为 Application Resource Bundle（ARB）格式的文件。

## 更改区域设置

默认情况下，当前区域设置为美国英语（en_US）。为 `defaultLocale` 赋予新值可以通过以下代码影响 Intl 库的所有方法：

```
Intl.defaultLocale = "fr_FR";
```

除了使用当前区域设置外，还有几种方式可以临时使用不同的区域设置，具体如下：

- 在调用 Intl 类的方法时直接指定区域设置
- 在创建格式化器类的实例时提供区域设置
- 使用 Intl 类的以下特殊 `withLocale` 方法：

```
static withLocale(String locale, Function message_function)
```

`withLocale` 方法的主要目的是延迟调用 `message_function` 函数，直到设置了正确的区域设置。`message_function` 函数可以是一个简单的消息函数、一个消息函数的包装器，或者一个操纵多个消息函数的复杂包装器。由于在静态分析时无法得知区域设置字符串，该方法会静默执行以下步骤：

1. 将指定的区域设置字符串与当前字符串互换。
2. 然后执行 `message_function` 函数并保存结果。
3. 将区域设置互换回来。
4. 最后返回 `message_function` 函数的结果。

## 国际化与本地化

作为第一个参数传递的消息字符串必须是简单表达式，其中只允许函数参数和花括号。其他参数必须是字面量，不应包含任何插值表达式。

> 消息函数的 `name` 和 `args` 参数是必需的，且 `name` 参数必须与调用函数的名称匹配。

现在，我们不将消息直接赋值给代码，而是调用 `hello` 函数为我们返回翻译后的消息，如下代码所示：

```
querySelector("#dart_greeting")
    ..text = hello('John');
```

在与示例类似的其他情况下，`hello` 函数可以将翻译后的消息结果与传入的参数列表进行插值。`examples` 和 `desc` 参数在运行时不会使用，仅提供给翻译人员。现在，我们准备开始使用消息函数，无需任何本地化即可在当前区域设置中获得正确的翻译。

## 添加复数形式

我们可以使用 Intl 类的 `plural` 方法，通过括号将单数和复数形式合并为一个字符串，如下代码所示：

```
String replace(int num, String str) => Intl.plural(
    num,
    zero: "No one occurrence replaced for $str",
    one: "$num occurrence replaced for $str",
    other: "$num occurrences replaced for $str",
    name: "replace",
    args: [num, str],
    desc: "How many occurrences replaced for string",
    examples: {'num':2, 'str':'hello'});
```

`plural` 方法会将 `zero`、`one` 和 `other` 参数的内容翻译并插值到本地化消息中。我们省略了 `two`、`few` 和 `many` 参数，因为一个方法只能组合 `one` 和 `other`，但如有必要你仍可指定它们。`plural` 方法作为字符串表达式表示时，可用作 `Intl.message` 的一部分，仅指定复数属性，如下代码所示：

```
String replace(int num, String str) => Intl.message(
    """${Intl.plural(
        num,
```

```
zero: "No one occurrence replaced for $str",
    one: "$num occurrence replaced for $str",
    two: "$num occurrence replaced for $str",
    few: "$num occurrences replaced for $str",
    other: "$num occurrences replaced for $str")}"",
    name: "replace",
    args: [num, str],
    desc:"How many occurrences replaced for string",
    examples: { 'num':2, 'str':'hello'},
);
```

## 添加性别

Intl 类的 `gender` 方法提供了开箱即用的基于性别的选择支持，如下代码所示：

```
String usage(String name, String gender, String car) =>
    Intl.gender(
        gender,
        male: "$name uses his $car",
        female: "$name uses her $car",
        other: "$name uses its car",
        name:"usage",
        args: [name, gender, car],
        desc: "A person uses the car.");
```

`gender` 参数必须等于以下字面量之一：`male`、`female` 或 `other`。该方法可用作 `Intl.message` 方法的一部分：

```
String usage(String name, String gender, String car) =>
    Intl.message(
        """${Intl.gender(
            gender,
            male: "$name uses his $car",
            female: "$name uses her $car",
            other: "$name uses its car")}""",
        name: "usage",
        args: [name, gender, car],
        desc: "A person uses the car.");
```

## 添加选择

最后但同样重要的是，`Intl` 类中的 `select` 方法用于根据可用选项以不同方式格式化消息：

```
String currencySelector(currency, amount) => Intl.select(currency,
    {
        "USD": "$amount United States dollars",
        "CDN" : "$amount Canadian dollars",
        "other" : "$amount some currency or other."
    },
    name: "currencySelector",
    args: [currency, amount],
    examples: {'currency': 'USD', 'amount': '20'},
    desc: "Translate abbreviation into full name of currency");
```

`select` 方法在大小写映射中查找 `currency` 的值，并返回找到的结果，否则返回空字符串。它可以是 `Intl.message` 方法的一部分，如下代码所示：

```
String currencySelector(currency, amount) => Intl.message(
    """${Intl.select(currency,
        {
            "USD": "$amount United States dollars",
            "CDN" : "$amount Canadian dollars",
            "other" : "$amount some currency or other."
        })}""",
    name: "currencySelector",
    args: [currency, amount],
    examples: {'currency': 'USD', 'amount': '20'},
    desc: "Translate abbreviation into full name of currency");
```

## 创建复杂的消息翻译

`message`、`plural`、`gender` 和 `select` 方法可以相互组合，以创建复杂的消息翻译，如下代码所示：

```
String currencySelector(currency, amount) => Intl.select(currency,
    {
        "USD": """${Intl.plural(amount,
            one: '$amount United States dollar',
            other: '$amount United States dollars')}""",
        "CDN": """${Intl.plural(amount,
            one: '$amount Canadian dollar',
            other: '$amount Canadian dollars')}"""
```

## 国际化与本地化

```
```html
<td><input type="text" id="firstName" name="firstName"></td>
</tr>
<tr>
    <td><label for="lastName">Last Name:</label></td>
    <td><input type="text" id="lastName" name="lastName"></td>
</tr>
<tr>
    <td><label>Gender:</label></td>
    <td>
        <input type="radio" name="sex" value="male">
        <span>Male</span>
        <input type="radio" name="sex" value="female">
        <span>Female</span>
    </td>
</tr>
<tr>
    <td colspan="2"><input type="submit" value="Register"></td>
</tr>
<tr>
    <td colspan="2">
        <span dir="RTL">Copyright 2014 עמוד ושיתוף</span>
    </td>
</tr>
</table>
</form>
```
```

以下截图显示了上述代码在 Web 浏览器中的结果：

![](img/52c05b53a56abd53584e4fed738ed509_426_0.png)

