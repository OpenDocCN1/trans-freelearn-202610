
该文件显示了使用 $_SERVER 全局数组访问文件的方法。然后它遍历全局 $_GET 和 $_POST 数组，并输出这些数组中的任何项目。

示例代码中包含一个名为 exampleREST.php 的示例 REST 客户端页面，它将与此服务交互。使用我们创建的 REST Parser 类，我们在页面顶部放置了一些测试代码，就像我们测试 XML-RPC 类时所做的那样。此代码将实例化 Parser 并创建两个数组。我们将使用第一个数组通过 POST 调用服务，使用第二个数组通过 GET 调用服务。

```php
<?php
    $params1 = array("One" => 1,  "Two" => "2 and 2 = 4");
    $params2 = array("Three" => 3, "Four" => "4 o'clock");
    $parser = new RESTParser();
    echo $parser->callService($params1, 'localhost', '/mashups/ch1/
        RESTService.php', 'POST');
    echo "<hr>";
    echo $parser->callService($params2, 'localhost', '/mashups/ch1/
        RESTService.php', 'GET');
    class RESTParser {
    /* Rest of RESTParser Class */
```
在此代码中，以“1”结尾的变量与 POST 请求相关联，以“2”结尾的任何内容都与 GET 请求相关联。为简洁起见，我们只查看其中一组的相关行。

```php
$params1 = array("One"=>1,  "Two"=>"2 and 2 = 4");
```
这段代码首先创建一个数组并将其分配给 $params1。

接下来，调用 callService()，启动事件链，它将数组作为 REST 请求的参数传递，以及服务器名称、服务的路径和我们希望使用的方法。callService() 的返回数据是 REST 服务返回的任何内容。我们只是输出返回值。

就像你为 XML-RPC 解析器类所做的那样，将此文件保存在你的 Web 服务器可提供服务的区域中。用你的 Web 浏览器打开此客户端页面。

你应该会看到以下内容：

![](img/c637937d44400f1da21fb7cca101de80_63_0.png)

水平规则分隔了两个 REST 请求。上半部分是使用 `createPostRequest()` 的 REST 调用的结果。服务遍历了 `POST` 变量并输出了它们。没有 `GET` 变量，因此该部分为空。在下半部分，情况相反。我们使用 `createGetRequest()` 并使用完全不同的变量集。服务找到了 `GET` 参数并输出了它们，但没有找到任何要输出的 `POST` 变量。

最后我们拥有了一个调用和存储 REST 服务的工具。是时候将 REST 结果处理成可在 PHP 中使用和操作的形式了。

## 处理 REST 响应

如果响应是一个简单的未格式化的文本字符串，我们可以使用我们声明并用来存储 `sendResult()` 返回值的变量轻松操作它。然而，REST API 很少会发送如此简单的内容。相反，它们更可能发送 XML 格式的响应。我们需要使用 PHP 加载 XML 文件，遍历它，提取我们需要的数据，并将其转换为 PHP 变量。

PHP 的工具箱中有一些解析器可用于处理原始 XML。它们可以分为 SAX-based 解析器或 DOM-based 解析器。SAX，即 Simple API for XML，是事件驱动的。解析器从 XML 流的开头开始，并在发生某些事件时执行 **回调函数**，例如元素的开始、元素值流的开始或元素的结束。回调函数是在某些事件发生时自动调用的函数。

当解析器经过 XML 文档中的任何一点时，它无法重新访问该点。如果它想这样做，就必须重新加载整个文档。

**DOM，文档对象模型处理，** 工作方式完全不同。解析器加载并在内存中存储整个 XML 文档。它将文档建模为元素的层次结构，因此 DOM-based 解析器也被称为树-based 解析器。我们可以使用某些 PHP 函数从文档的任何位置操作和提取信息。虽然这给了我们更大的灵活性，但将 XML 文档保存在内存中是资源密集型的。此外，为了从 DOM 中提取信息，PHP 在对数据进行任何操作之前必须在文档中进行某种搜索。在 SAX 中，没有搜索开销。解析器只需遍历文档并将数据交给其他函数处理。由于这些因素，DOM-based 解析被认为比 SAX-based 解析更慢、更耗资源。

这并不意味着树-based 处理没有其用武之地。能够在任何时候搜索文档而无需重新加载的开销是无价的，在你的职业生涯中 sooner or later 你会需要它。你为项目使用的哪种方法完全取决于你的应用程序以及你如何架构它。在未来的项目中，我们将探索 PHP 和 DOM 解析。现在，在本项目中，我们将使用 PHP 的 SAX 实现。

SAX 函数记录在 `http://www.php.net/manual/en/ref.xml.php`。这些函数有幸成为创建用于解析 XML 的第一组函数，自 PHP 3 以来就一直存在。默认启用，你无需重新编译 PHP 即可使用它们，除非你的 PHP 构建在编译时明确使用 `--disable-xml` 开关禁用了它们。

## 使用 PHP 和 SAX 的基本演练

虽然 PHP 的 XML 系列中有很多函数，但使用 PHP 进行基本的 SAX 处理只需六个步骤：

- 1. 创建一个 SAX 解析器。
- 2. 如果需要任何解析器选项，请设置它们。
- 3. 声明在遇到元素开始时将被调用的回调函数，以及在遇到结束元素时将被调用的函数。
- 4. 声明在遇到字符数据或元素之间的数据时将被调用的回调函数。
- 5. 将文档馈送到解析器中。
- 6. 释放解析器。

让我们用一个非常简单的脚本来看一下这个过程。此脚本名为 exampleSAX.php。

```php
<?php
    $xml = "<Pet>" .
          "  <Name id='4323412'>Avi</Name>" .
          "  <Species>African Grey</Species>" .
          "  <Gender>F</Gender>" .
          "</Pet>";
    $parser = xml_parser_create();
    xml_parser_set_option($parser, XML_OPTION_CASE_FOLDING, 0);
    xml_parser_set_option($parser, XML_OPTION_TARGET_ENCODING,
                          "UTF-8");
    xml_set_element_handler($parser, "startElement", "endElement");
    xml_set_character_data_handler($parser, "characterData");
    xml_parse($parser, $xml);
    function startElement($parser, $name, $attributes) {
        echo "Start: " . $name . "<br />";
        foreach ($attributes as $key => $value) {
            echo "Attribute: " . $key . " :: " . $value . "<br />";
        }
    }
    function endElement($parser, $name) {
        echo "End:" . $name . "<br>";
    }
    function characterData($parser, $data) {
        echo "Data:" . $data . "<br>";
    }
?>
```

在此脚本开始时，我们设置了一个模拟的 XML 文档片段并将其设置为变量 $xml。之后，我们可以开始 SAX 解析过程。

## 使用 PHP 的 XML 函数

我们要做的第一件事是使用函数 `xml_parser_create()` 创建一个解析器。此函数为我们返回一个 SAX 解析器，我们将在整个过程中使用它。

```
$parser = xml_parser_create();
```

要设置任何解析器选项，我们使用 `xml_parser_set_option()`。

```
xml_parser_set_option($parser, XML_OPTION_CASE_FOLDING, 0);
```

有四个选项可以更改：

| Option | Value | Description |
|---|---|---|
| XML_OPTION_CASE_FOLDING | 1 or 0 | 控制元素名称是否传递给回调函数。默认启用（1）。 |
| XML_OPTION_SKIP_TAGSTART | Integer | 指定在标签开头应跳过的字符数。默认值为零。 |
| XML_OPTION_SKIP_WHITE | 1 or 0 | 控制是否跳过仅包含空白的数据。默认禁用（0）。 |
| XML_OPTION_TARGET_ENCODING | String | 指定要使用的编码。支持 *ISO-8859-1*、*US-ASCII* 和 *UTF-8*。默认情况下，它是创建解析器时使用的编码。 |

中间两个选项在 PHP 4 和 5 以及 UNIX 和 Windows 版本的 PHP 之间表现出一些不一致的行为。使用时请谨慎。

第一个和最后一个选项是稳定的，而且你会不时遇到需要它们的情况。我们应该快速看一下它们。我们稍后会看到开始元素和结束元素回调函数如何接收元素的名称。我们需要在回调函数中测试元素的值。PHP 的 SAX 解析器在将元素名称传递给函数时会执行 **大小写折叠**。换句话说，元素名称的字母大小写会作为大写传递给回调函数。然而，XML 是区分大小写的。PHP 的这种默认行为有点有争议，因为 PHP 将元素名称视为全大写，而 XML 认为 `elementName` 和 `ELEMENTNAME` 是不同的。有些人认为在解析器中关闭大小写折叠是良好的编码实践。
