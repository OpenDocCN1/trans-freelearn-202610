
在非 WSDL 模式下，你无需请求 WSDL 文档，也没有解析开销。控制是另一个原因。在非 WSDL 模式下，对于某些高级操作，你可能需要处理更细粒度的任务。有些人喜欢这样。

非 WSDL 模式还提供了一些其他情况下不可用的实例化选项。不过主要原因可能是你根本没有选择。WSDL 文件可能对你不可公开获取。为了让你的生活更轻松，我建议在可能的情况下始终使用 WSDL 模式。然而，在非 WSDL 模式下工作完全可以接受，在实践中也不罕见。我们将介绍如何使用这两种模式进行调用，并且稍后在实际的 mashup 中，我们将对其中一个调用使用非 WSDL。

## 在 WSDL 模式下实例化

是否使用 WSDL 模式还是非 WSDL 模式的 SoapClient 取决于你最初如何实例化 SoapClient。让我们来看一下该类的构造函数：

```
class SoapClient {
  __construct ( mixed wsdl [, array options] )
}
```

传递给它的第一个参数是 WSDL。通常这是一个指向 WSDL 文件的简单 URI，尽管 mixed 类型允许你将存储在字符串中的 WSDL 文件传递到此参数。第二个参数是一个可选的选项数组。

如果你想进入 WSDL 模式，只需将 WSDL 传递给第一个参数即可。

```
$client = new SoapClient(
'http://webservices.daelab.net/temperatureconversions/TemperatureConversions.wso?WSDL'
);
```

使用此对象进行的所有后续操作都将在 WSDL 模式下进行。

## 在非 WSDL 模式下实例化

创建非 WSDL 客户端稍微复杂一些。首先，wsdl 参数无关紧要。你必须向此选项传递 null 值。这也意味着你的客户端不会自动知道服务操作点在哪里。你必须手动告诉它。为此，在实例化时在选项数组中设置服务的位置和目标命名空间。换句话说，这个可选参数不再是可选的。这分别通过数组的 location 和 uri 元素完成。

```
$client = new SoapClient(
    null,
    array(
        'location' => 'http://webservices.daelab.net/temperatureconversions/TemperatureConversions.wso',
        'uri' => 'http://webservices.daelab.net/temperature'
    )
);
```

我们从哪里获取 URL 的 location 值？它是从 service 元素中获取的。它是端口的子 address 元素。uri 值也取自 WSDL，但位于最顶部。它是 definitions 元素的 targetNamespace 值。在本例中，它们是相同的值，但在许多实现中它们并不相同。

![](img/c637937d44400f1da21fb7cca101de80_121_0.png)

### 使用其他选项

> 你可能想查看 options 数组的文档，尽管大多数选项在大多数基本调用中并不需要，尤其是在 WSDL 模式下。它们都列在 SoapClient 构造函数页面 http://php.net/manual/function.soap-soapclient-construct.php 中。其中一个特定的选项 trace 非常有用。如果设置为 true，这将允许我们在稍后检查实际的 SOAP 信封，并进行一般调试。尽管如此，了解其他选项也很好，尤其是当你遇到 SOAP 服务的奇怪通信问题时。解决方案可能是在 options 数组中的一个设置。

这两种选项中的任何一种都会为我们提供一个名为 $client 的新的 SoapClient 对象。我们现在可以使用该对象的方法。

## 进行调用和使用 SoapClient 方法

在进行 SOAP 调用之前，让我们先看看可用的一些方法。如果你使用的是 WSDL 模式，两个方法会特别有用，__getFunctions 和 __getTypes。

请记住，当我们创建 SoapClient 对象时，PHP 已经查询了 WSDL。它不仅仅在心里记下 WSDL。它会读取它，并通过将操作翻译成 SoapClient 对象本身中的可用方法来让我们的生活更轻松。（我们稍后会看到这如何影响我们进行 SOAP 调用方式。）这样做时，它显然保存了可用函数和数据类型的列表。

要访问这些列表，请对 SoapClient 使用 __getFunctions 以返回 WSDL 中所有 SOAP 操作及其参数的数组。__getTypes 返回 WSDL 中使用的所有数据类型的数组。如果没有文档，这非常有用。

我们可以在 URL 提取服务上看到它的实际效果：

```
<?php
    $client = new SoapClient(
    'http://webservices.daelab.net/temperatureconversions/
                TemperatureConversions.wso?WSDL'
    );
?>
<h1>Types:</h1>
<?= var_dump($client->__getTypes()); ?>
<h1>Functions:</h1>
<?= var_dump($client->__getFunctions()); ?>
```

这段代码在示例中作为 SoapClientTest.php。运行该页面将在页面源中给出此输出：

```
<h1>Types:</h1>
array(8) {
    [0]=>
    string(49) "struct CelciusToFahrenheit {
    decimal nCelcius;
}"
    [1]=>
    string(74) "struct CelciusToFahrenheitResponse {
    decimal CelciusToFahrenheitResult;
}"
    [2]=>
    string(52) "struct FahrenheitToCelcius {
    decimal nFahrenheit;
}"
...
<h1>Functions:</h1>
array(4) {
    [0]=>
    string(80) "CelciusToFahrenheitResponse CelciusToFahrenheit(CelciusT
                    oFahrenheit $parameters)"
    [1]=>
    string(80) "FahrenheitToCelciusResponse FahrenheitToCelcius(Fahrenhe
    itToCelcius $parameters)"
    [2]=>
string(77) "WindChillInCelciusResponse WindChillInCelcius(
                    WindChillInCelcius $parameters)"
[3]=>
string(86) "WindChillInFahrenheitResponse WindChillInFahrenheit(
                    WindChillInFahrenheit $parameters)"
}
```

方法签名的格式非常像 C 风格。它对输入和返回使用对象提示。列出的第一个函数是 CelciusToFahrenheit。它接受一个 CelciusToFahrenheit 对象。

查看类型，我们看到 CelciusToFahrenheit 是一个仅包含一个元素的 struct，即一个名为 nCelcius 的 decimal。如我们在 WSDL 中发现的那样，这告诉我们需要向 CelciusToFahrenheit 操作传递一个名为 nCelcius 的参数。

返回值是一个 CelciusToFahrenheitResponse 对象。查看类型，我们将该对象追溯到一个名为 CelciusToFahrenheitResponse 的 struct。这是一个 struct，给出了以下定义：

```
string(74) "struct CelciusToFahrenheitResponse {
    decimal CelciusToFahrenheitResult;
```

最后，我们看到 CelciusToFahrenheitResponse 基本上是一个名为 CelciusToFahrenheitResult 的 decimal。你可以在 WSDL 中查看是否有更多细节。或者，由于你已经知道需要哪些参数，你可以直接进行 SOAP 调用并使用 SoapClient 检查结果。

你如何进行 WSDL 调用取决于你处于 WSDL 模式还是非 WSDL 模式。除了访问某些函数之外，这是唯一根据 WSDL 或非 WSDL 而变化的语法区域。

## 在 WSDL 模式下调用 SOAP 操作

在 WSDL 模式下，PHP 已将操作内部化并使其成为 SoapClient 对象的扩展。它们的使用极其简单。你可以将这些操作视为 SoapClient 对象的方法。

```
$params = array('nCelcius' => 37);
$client = new SoapClient(
'http://webservices.daelab.net/temperatureconversions/
TemperatureConversions.wso?WSDL',
array('trace' => true)
);
$result = $client->CelciusToFahrenheit($params);
```

## 在非 WSDL 模式下调用 SOAP 操作

在非 WSDL 模式下，SoapClient 不知道有哪些方法可用。为此，SoapClient 持有名为 __soapCall 的操作调用方法。此方法的唯一存在是为了针对服务启动 SOAP 调用，并且它接受两个参数。第一个是 SOAP 操作的名称。第二个是你希望传递给服务的 SOAP 参数的数组。重写上述调用在非 WSDL 模式下，我们得到以下内容：

```
$params = array('nCelcius' => 37);
$client = new SoapClient(
null,
array(
'location' => 'http://webservices.daelab.net/temperatureconversions/TemperatureConversions.wso',
'uri' => 'http://webservices.daelab.net/temperature'
)
);
$result = $client->__soapCall('CelciusToFahrenheit', $params);
```

请记住，我们需要在实例化 SoapClient 以进入非 WSDL 模式时提供 location 和 uri 选项，并且将第一个参数传递为 null。之后，我们可以使用 __soapCall 来调用该操作。

这两种方法中的任何一种都会将 SOAP 响应放入名为 $result 的变量中。现在，我们可以使用它来查看服务器返回给我们什么。

## 处理 SOAP 响应

在开始显示结果之前，我们需要看看它是否成功。

## 使用 SoapFault 处理 SOAP 错误

SoapClient 利用 PHP 5 中的新 try/catch 块来处理错误。如果出现问题，无论是我们的原因还是服务器返回了故障，SoapClient 都会抛出 SoapFault 对象。我们需要捕获它并优雅地处理错误。在以下示例中，我故意将方法名更改为 GetURLs。因为该方法不存在，所以它会失败。

```
try {
    $result = $client->ConvertTemp($params);
}
catch (SoapFault $e) {
    echo "Error!<br>";
```
