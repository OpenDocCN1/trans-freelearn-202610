
另一个重要的选项是目标编码选项。默认情况下，SAX 解析器在调用 xml_create_parser() 时会将编码设置为脚本的编码。如果您处理的 XML 文档的编码不同，则需要使用 XML_OPTION_TARGET_ENCODING 来切换它。将您希望设置的编码作为 xml_parser_set_option() 的第三个参数传递。

```
xml_parser_set_option($parser, XML_OPTION_TARGET_ENCODING,
"UTF-8");
```

在更改选项后，我们可以为开始和结束元素设置回调函数。我们使用 xml_set_element_handler() 函数来完成此操作。

```
xml_set_element_handler($parser, "startElement", "endElement");
```

此函数接受三个参数。第一个是我们创建的解析器。第二个是开始元素回调函数的名称，第三个是结束元素回调函数的名称。有关 PHP 回调的更多说明，请参阅 http://uk.php.net/callback。

最后，在开始解析之前，我们需要设置另一个回调函数。我们需要指定当解析器遇到字符数据时将被调用的函数。我们使用 xml_set_character_data_handler() 来完成此操作。

```
xml_set_character_data_handler($parser, "characterData");
```

此函数的工作方式与 xml_set_element_handler 非常相似。它将解析器作为第一个参数，将回调函数的名称作为第二个参数。

在我们的示例脚本中，我们还定义了回调函数 startElement()、endElement() 和 characterData()。我们稍后会查看它们。只要它们在脚本中已定义且可用，您就可以使用 xml_parse() 开始解析 XML。

```
xml_parse($parser, $xml);
```

将解析器和 XML 作为参数传递给 xml_parse()。

## 设置回调函数

我们设置的回调函数的唯一作用是显示元素的名称、任何属性以及元素中存在的任何数据。函数 get 这些值 from 参数 passed to it by PHP, when the callback functions are called. 如果您熟悉面向对象编程，可以将回调函数的概念视为实现接口。当我们创建回调函数时，必须在方法签名中声明这些参数。让我们看看我们定义的每个回调函数的具体情况以及它们如何使用各个参数。

我们的开始元素回调仅显示元素名称并遍历元素拥有的任何属性。

```
function startElement($parser, $name, $attributes) {
    echo "Start: " . $name . "<br />";
    foreach ($attributes as $key => $value) {
        echo "Attribute: " . $key . " :: " . $value . "<br />";
    }
}
```

PHP 将解析器、元素的名称以及属性作为数组传递给开始元素回调。如果元素有任何属性，它们将作为关联数组在第三个参数中传递给回调。在这里，我们只是遍历该数组。

结束元素回调接收两个参数——解析器以及再次出现的元素名称。

```
function endElement($parser, $name) {
    echo "End:" . $name . "<br>";
}
```

我们的最后一个回调函数是在解析器遇到字符数据时调用的函数，即不是元素的数据。

```
function characterData($parser, $data) {
    echo "Data:" . $data . "<br>";
}
```

解析器作为第一个参数传递，数据的值作为第二个参数传递。我们再次简单地输出数据。

## 回调函数的实际运行

现在是时候查看脚本的实际运行情况以及回调函数的调用方式了。将脚本保存到可由 Web 服务器提供服务的区域。用 Web 浏览器访问该脚本。

您应该会看到以下内容：

![](img/c637937d44400f1da21fb7cca101de80_69_0.png)

在我们逐步分析时，请回想一下我们传递的 XML 文档的结构。有一个 Pet 元素，其中嵌套了三个元素（Name、Species、Gender），它们都包含数据。

事件和函数执行如下：

- 1. 遇到 Pet 元素，触发 startElement()。
- 2. 形如 Pet 之后空白的字符数据执行 characterData()。characterData 的 echo 语句仅为空白。
- 3. 遇到 Name 元素，触发 startElement()。该元素具有属性，开始元素回调的 $attributes 值是一个数组。
- 4. startElement() 内部的 foreach 循环被执行，Name 元素的属性值被遍历并输出。
- 5. Name 的值由于 characterData() 而出现，因为这次字符数据不仅仅是空白。
- 6. 在 Name 元素关闭时，调用 endElement()。
- 7. 对于 Species 和 Gender 元素，startElement()、characterData()、endElement() 序列再次执行。
- 8. 最后，在 Pet 元素关闭时调用 endElement()。

## 创建 SAX 解析器类

关于 SAX 需要注意的重要一点是，当遇到开始元素、字符数据或结束元素时，只有一个且仅有一个函数被激活。这有几个含义：

- 1. 当解析器遍历文档时，我们必须将我们关心的数值存储在更“全局”的变量中。它不能存储在回调函数的局部变量中，因为函数执行后它们会消失。
- 2. 我们必须检测要用作存储的“全局”变量。唯一的方法是在开始元素回调函数被调用时检查元素名称。我们在检查元素名称时也可以检测元素属性。
- 3. 由于我们必须在开始元素回调中比较元素名称，任何 SAX 解析器都高度定制于其解析的 XML 文档。对 XML 文档的任何更改都可能导致我们创建的任何 SAX 解析器需要更改。我们应尽力最小化必要的代码维护，因为解析器和 XML 文档紧密耦合。

创建一个解析器类来处理我们的 SAX 处理是满足需求并使用 SAX 事件驱动解析方法的一种简单高效的方式。此外，利用 PHP 5 的新面向对象特性，我们可以设计出具有一定灵活性的架构。

我们并不是要创建一个单一的解析器类，而是要创建两个类。第一个是抽象类。构造函数将声明解析器、设置选项并执行解析。开始元素、结束元素和字符数据回调函数的需要根据单个 XML 文档进行定制，因此它们将在此类中为抽象。

我们的第二个类将扩展此抽象类。第二个类将使用其自己的专用代码实现父类的抽象类。此类的属性是我们希望从 XML 文档中提取的值。这使我们能够将特定于 XML 文档的代码与通用代码分离。如果我们的应用程序需要解析更多 XML 文档，我们不必重写通用代码。我们只需扩展抽象类，加载和设置代码就会为我们准备好。虽然这听起来很复杂，但我们可以重构我们之前的示例代码。

```
abstract class SAXParser {
    private $parser;
    protected $tagName = "";
```

```
abstract public function startElement($parser, $name, $attributes);
abstract public function endElement($parser, $name);
abstract public function characterData($parser, $data);
protected function parse($xml) {
    xml_parse($this->parser, $xml);
}
public function __construct($xml) {
    $this->parser = xml_parser_create();
    xml_set_object($this->parser, $this);
    xml_parser_set_option($this->parser, XML_OPTION_CASE_FOLDING, 0);
    xml_set_element_handler($this->parser, "startElement",
        "endElement");
    xml_set_character_data_handler($this->parser, "characterData");
}
}
class PetParser extends SAXParser {
    public $petName;
    public $petId;
    public function startElement($parser, $name, $attributes) {
        if ($name == "Name") {
            $this->tagName = $name;
            $this->petId = $attributes['id'];
        }
    }
    public function characterData($parser, $data) {
        if ($this->tagName == "Name") {
            $this->petName = $data;
        }
    }
    public function endElement($parser, $name) {
        $this->tagName = "";
    }
}
```

## 检查类

我们的抽象类名为 SAXParser。我们创建的任何解析器类都将实现 SAXParser。SAXParser 有两个属性：tagName 和 parser。parser 是 PHP 使用 xml_parser_create 创建的 XML 解析器。通过将其作为对象，我们可以将解析器的创建和设置与实际的 XML 解析分离。我们将在查看构造函数和 parse 抽象方法时稍后看到这一点。tagName 是我们用来存储和检查元素名称的容器。当我们是
