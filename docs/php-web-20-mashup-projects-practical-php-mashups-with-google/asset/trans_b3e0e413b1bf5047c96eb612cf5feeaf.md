
请记住，FROM 子句在 SPARQL 查询中不使用。你在此处传递的文件实际上是真正的 RDF 源。能够加载远程文件显然意味着我们可以在所有基于 RDF 的 mashup 上使用此库，并且可以在运行时获取 RDF 数据。

在此之后，我们可以创建查询并执行它。

```
$query = '
PREFIX mypets: <http://www.shuchow.com/>
SELECT ?age
FROM <http://www.shuchow.com/mashups/ch6/pets.rdf>
WHERE {
    ?Description mypets:name "Saffy" .
    ?Description mypets:age ?age
}';

$result = $sparqlClient->sparqlQuery($query);
if ($result != false) {
    foreach ($result as $cat) {
        if ($cat != "") {
            echo "Age: " . $cat['?age']->getLabel();
        }
    }
}
```

在此块中，我们将 SPARQL 查询放入名为 $query 的变量中。我们将其传递给 sparqlQuery 方法。此方法位于默认模型中。它接受一个 SPARQL 查询并在内存中的 RDF 文件上执行它。其返回值是一个对象数组。每个数组中的键是我们添加到查询的 SELECT 子句中的变量，包括问号。这些是 RAP 库中的 Resources 对象。Resources 对象中的 getLabel() 方法返回变量的值。

要获取多个变量，我们只需在 foreach 循环中使用其他键。

```
$query = '
PREFIX mypets: <http://www.shuchow.com/>
SELECT ?name ?age
FROM <http://www.shuchow.com/mashups/ch6/pets.rdf>
WHERE {
    ?Description mypets:name ?name
    ?Description mypets:age ?age
}
LIMIT 5
';
```

$result = $sparqlClient->sparqlQuery($query);

if ($result != false) {
    foreach ($result as $cat) {
        if ($cat != "") {
            echo "Name: " . $cat['?name']->getLabel() . ", Age: " .
                $cat['?age']->getLabel() . "<br />";
        }
    }
}
```

运行此代码会在屏幕上产生以下输出：

```
Name: Snowball, Age: 14
Name: Lizzie Borden, Age: 14
Name: Saffy, Age: 10
Name: Pim Pim, Age: 12
Name: Tera-San, Age: 6
```

RAP 是一个相当强大的工具。我们只使用了其功能的一小部分。如果 RDF 是你应用程序的重要组成部分，那么探索这个庞大的库绝对是值得的。

## XMLHttpRequest 对象

我们接下来要查看的技术与我们使用过的面向服务器的工具不同。你可能听说过 AJAX，即异步 JavaScript 和 XML 传输。至少，你可能在 Google Mail 和 Yahoo! Mail 等网站上见过它。AJAX 允许 Web 浏览器在不刷新页面的情况下与服务器交互。与动态 HTML 结合，它在用户和网站之间创造了新的交互水平。在用户面前几乎瞬时的[content]
请记住，FROM 子句在 SPARQL 查询中不使用。你在此处传递的文件实际上是真正的 RDF 源。能够加载远程文件显然意味着我们可以在所有基于 RDF 的 mashup 上使用此库，并且可以在运行时获取 RDF 数据。

在此之后，我们可以创建查询并执行它。

```
$query = '
PREFIX mypets: <http://www.shuchow.com/>
SELECT ?age
FROM <http://www.shuchow.com/mashups/ch6/pets.rdf>
WHERE {
    ?Description mypets:name "Saffy" .
    ?Description mypets:age ?age
}';

$result = $sparqlClient->sparqlQuery($query);
if ($result != false) {
    foreach ($result as $cat) {
        if ($cat != "") {
            echo "Age: " . $cat['?age']->getLabel();
        }
    }
}
```

在此块中，我们将 SPARQL 查询放入名为 $query 的变量中。我们将其传递给 sparqlQuery 方法。此方法位于默认模型中。它接受一个 SPARQL 查询并在内存中的 RDF 文件上执行它。其返回值是一个对象数组。每个数组中的键是我们添加到查询的 SELECT 子句中的变量，包括问号。这些是 RAP 库中的 Resources 对象。Resources 对象中的 getLabel() 方法返回变量的值。

要获取多个变量，我们只需在 foreach 循环中使用其他键。

```
$query = '
PREFIX mypets: <http://www.shuchow.com/>
SELECT ?name ?age
FROM <http://www.shuchow.com/mashups/ch6/pets.rdf>
WHERE {
    ?Description mypets:name ?name
    ?Description mypets:age ?age
}
LIMIT 5
';
```

$result = $sparqlClient->sparqlQuery($query);

if ($result != false) {
    foreach ($result as $cat) {
        if ($cat != "") {
            echo "Name: " . $cat['?name']->getLabel() . ", Age: " .
                $cat['?age']->getLabel() . "<br />";
        }
    }
}
```

运行此代码会在屏幕上产生以下输出：

```
Name: Snowball, Age: 14
Name: Lizzie Borden, Age: 14
Name: Saffy, Age: 10
Name: Pim Pim, Age: 12
Name: Tera-San, Age: 6
```

RAP 是一个相当强大的工具。我们只使用了其功能的一小部分。如果 RDF 是你应用程序的重要组成部分，那么探索这个庞大的库绝对是值得的。

## XMLHttpRequest 对象

我们接下来要查看的技术与我们使用过的面向服务器的工具不同。你可能听说过 AJAX，即异步 JavaScript 和 XML 传输。至少，你可能在 Google Mail 和 Yahoo! Mail 等网站上见过它。AJAX 允许 Web 浏览器在不刷新页面的情况下与服务器交互。与动态 HTML 结合，它在用户和网站之间创造了新的交互水平。在用户面前几乎瞬时的[content]
请记住，FROM 子句在 SPARQL 查询中不使用。你在此处传递的文件实际上是真正的 RDF 源。能够加载远程文件显然意味着我们可以在所有基于 RDF 的 mashup 上使用此库，并且可以在运行时获取 RDF 数据。

在此之后，我们可以创建查询并执行它。

```
$query = '
PREFIX mypets: <http://www.shuchow.com/>
SELECT ?age
FROM <http://www.shuchow.com/mashups/ch6/pets.rdf>
WHERE {
    ?Description mypets:name "Saffy" .
    ?Description mypets:age ?age
}';

$result = $sparqlClient->sparqlQuery($query);
if ($result != false) {
    foreach ($result as $cat) {
        if ($cat != "") {
            echo "Age: " . $cat['?age']->getLabel();
        }
    }
}
```

在此块中，我们将 SPARQL 查询放入名为 $query 的变量中。我们将其传递给 sparqlQuery 方法。此方法位于默认模型中。它接受一个 SPARQL 查询并在内存中的 RDF 文件上执行它。其返回值是一个对象数组。每个数组中的键是我们添加到查询的 SELECT 子句中的变量，包括问号。这些是 RAP 库中的 Resources 对象。Resources 对象中的 getLabel() 方法返回变量的值。

要获取多个变量，我们只需在 foreach 循环中使用其他键。

```
$query = '
PREFIX mypets: <http://www.shuchow.com/>
SELECT ?name ?age
FROM <http://www.shuchow.com/mashups/ch6/pets.rdf>
WHERE {
    ?Description mypets:name ?name
    ?Description mypets:age ?age
}
LIMIT 5
';
```

$result = $sparqlClient->sparqlQuery($query);

if ($result != false) {
    foreach ($result as $cat) {
        if ($cat != "") {
            echo "Name: " . $cat['?name']->getLabel() . ", Age: " .
                $cat['?age']->getLabel() . "<br />";
        }
    }
}
```

运行此代码会在屏幕上产生以下输出：

```
Name: Snowball, Age: 14
Name: Lizzie Borden, Age: 14
Name: Saffy, Age: 10
Name: Pim Pim, Age: 12
Name: Tera-San, Age: 6
```

RAP 是一个相当强大的工具。我们只使用了其功能的一小部分。如果 RDF 是你应用程序的重要组成部分，那么探索这个庞大的库绝对是值得的。

## XMLHttpRequest 对象

我们接下来要查看的技术与我们使用过的面向服务器的工具不同。你可能听说过 AJAX，即异步 JavaScript 和 XML 传输。至少，你可能在 Google Mail 和 Yahoo! Mail 等网站上见过它。AJAX 允许 Web 浏览器在不刷新页面的情况下与服务器交互。与动态 HTML 结合，它在用户和网站之间创造了新的交互水平。在用户面前几乎瞬时的[content]
请记住，FROM 子句在 SPARQL 查询中不使用。你在此处传递的文件实际上是真正的 RDF 源。能够加载远程文件显然意味着我们可以在所有基于 RDF 的 mashup 上使用此库，并且可以在运行时获取 RDF 数据。

在此之后，我们可以创建查询并执行它。

```
$query = '
PREFIX mypets: <http://www.shuchow.com/>
SELECT ?age
FROM <http://www.shuchow.com/mashups/ch6/pets.rdf>
WHERE {
    ?Description mypets:name "Saffy" .
    ?Description mypets:age ?age
}';

$result = $sparqlClient->sparqlQuery($query);
if ($result != false) {
    foreach ($result as $cat) {
        if ($cat != "") {
            echo "Age: " . $cat['?age']->getLabel();
        }
    }
}
```

在此块中，我们将 SPARQL 查询放入名为 $query 的变量中。我们将其传递给 sparqlQuery 方法。此方法位于默认模型中。它接受一个 SPARQL 查询并在内存中的 RDF 文件上执行它。其返回值是一个对象数组。每个数组中的键是我们添加到查询的 SELECT 子句中的变量，包括问号。这些是 RAP 库中的 Resources 对象。Resources 对象中的 getLabel() 方法返回变量的值。

要获取多个变量，我们只需在 foreach 循环中使用其他键。

```
$query = '
PREFIX mypets: <http://www.shuchow.com/>
SELECT ?name ?age
FROM <http://www.shuchow.com/mashups/ch6/pets.rdf>
WHERE {
    ?Description mypets:name ?name
    ?Description mypets:age ?age
}
LIMIT 5
';
```

$result = $sparqlClient->sparqlQuery($query);

if ($result != false) {
    foreach ($result as $cat) {
        if ($cat != "") {
            echo "Name: " . $cat['?name']->getLabel() . ", Age: " .
                $cat['?age']->getLabel() . "<br />";
        }
    }
}
```

运行此代码会在屏幕上产生以下输出：

```
Name: Snowball, Age: 14
Name: Lizzie Borden, Age: 14
Name: Saffy, Age: 10
Name: Pim Pim, Age: 12
Name: Tera-San, Age: 6
```

RAP 是一个相当强大的工具。我们只使用了其功能的一小部分。如果 RDF 是你应用程序的重要组成部分，那么探索这个庞大的库绝对是值得的。

## XMLHttpRequest 对象

我们接下来要查看的技术与我们使用过的面向服务器的工具不同。你可能听说过 AJAX，即异步 JavaScript 和 XML 传输。至少，你可能在 Google Mail 和 Yahoo! Mail 等网站上见过它。AJAX 允许 Web 浏览器在不刷新页面的情况下与服务器交互。与动态 HTML 结合，它在用户和网站之间创造了新的交互水平。在用户面前几乎瞬时的[content]
请记住，FROM 子句在 SPARQL 查询中不使用。你在此处传递的文件实际上是真正的 RDF 源。能够加载远程文件显然意味着我们可以在所有基于 RDF 的 mashup 上使用此库，并且可以在运行时获取 RDF 数据。

在此之后，我们可以创建查询并执行它。

```
$query = '
PREFIX mypets: <http://www.shuchow.com/>
SELECT ?age
FROM <http://www.shuchow.com/mashups/ch6/pets.rdf>
WHERE {
    ?Description mypets:name "Saffy" .
    ?Description mypets:age ?age
}';

$result = $sparqlClient->sparqlQuery($query);
if ($result != false) {
    foreach ($result as $cat) {
        if ($cat != "") {
            echo "Age: " . $cat['?age']->getLabel();
        }
    }
}
```

在此块中，我们将 SPARQL 查询放入名为 $query 的变量中。我们将其传递给 sparqlQuery 方法。此方法位于默认模型中。它接受一个 SPARQL 查询并在内存中的 RDF 文件上执行它。其返回值是一个对象数组。每个数组中的键是我们添加到查询的 SELECT 子句中的变量，包括问号。这些是 RAP 库中的 Resources 对象。Resources 对象中的 getLabel() 方法返回变量的值。

要获取多个变量，我们只需在 foreach 循环中使用其他键。

```
$query = '
PREFIX mypets: <http://www.shuchow.com/>
SELECT ?name ?age
FROM <http://www.shuchow.com/mashups/ch6/pets.rdf>
WHERE {
    ?Description mypets:name ?name
    ?Description mypets:age ?age
}
LIMIT 5
';
```

$result = $sparqlClient->sparqlQuery($query);

if ($result != false) {
    foreach ($result as $cat) {
        if ($cat != "") {
            echo "Name: " . $cat['?name']->getLabel() . ", Age: " .
                $cat['?age']->getLabel() . "<br />";
        }
    }
}
```

运行此代码会在屏幕上产生以下输出：

```
Name: Snowball, Age: 14
Name: Lizzie Borden, Age: 14
Name: Saffy, Age: 10
Name: Pim Pim, Age: 12
Name: Tera-San, Age: 6
```

RAP 是一个相当强大的工具。我们只使用了其功能的一小部分。如果 RDF 是你应用程序的重要组成部分，那么探索这个庞大的库绝对是值得的。

## XMLHttpRequest 对象

我们接下来要查看的技术与我们使用过的面向服务器的工具不同。你可能听说过 AJAX，即异步 JavaScript 和 XML 传输。至少，你可能在 Google Mail 和 Yahoo! Mail 等网站上见过它。AJAX 允许 Web 浏览器在不刷新页面的情况下与服务器交互。与动态 HTML 结合，它在用户和网站之间创造了新的交互水平。在用户面前几乎瞬时的[content]
请记住，FROM 子句在 SPARQL 查询中不使用。你在此处传递的文件实际上是真正的 RDF 源。能够加载远程文件显然意味着我们可以在所有基于 RDF 的 mashup 上使用此库，并且可以在运行时获取 RDF 数据。

在此之后，我们可以创建查询并执行它。

```
$query = '
PREFIX mypets: <http://www.shuchow.com/>
SELECT ?age
FROM <http://www.shuchow.com/mashups/ch6/pets.rdf>
WHERE {
    ?Description mypets:name "Saffy" .
    ?Description mypets:age ?age
}';

$result = $sparqlClient->sparqlQuery($query);
if ($result != false) {
    foreach ($result as $cat) {
        if ($cat != "") {
            echo "Age: " . $cat['?age']->getLabel();
        }
    }
}
```

在此块中，我们将 SPARQL 查询放入名为 $query 的变量中。我们将其传递给 sparqlQuery 方法。此方法位于默认模型中。它接受一个 SPARQL 查询并在内存中的 RDF 文件上执行它。其返回值是一个对象数组。每个数组中的键是我们添加到查询的 SELECT 子句中的变量，包括问号。这些是 RAP 库中的 Resources 对象。Resources 对象中的 getLabel() 方法返回变量的值。

要获取多个变量，我们只需在 foreach 循环中使用其他键。

```
$query = '
PREFIX mypets: <http://www.shuchow.com/>
SELECT ?name ?age
FROM <http://www.shuchow.com/mashups/ch6/pets.rdf>
WHERE {
    ?Description mypets:name ?name
    ?Description mypets:age ?age
}
LIMIT 5
';
```

$result = $sparqlClient->sparqlQuery($query);

if ($result != false) {
    foreach ($result as $cat) {
        if ($cat != "") {
            echo "Name: " . $cat['?name']->getLabel() . ", Age: " .
                $cat['?age']->getLabel() . "<br />";
        }
    }
}
```

运行此代码会在屏幕上产生以下输出：

```
Name: Snowball, Age: 14
Name: Lizzie Borden, Age: 14
Name: Saffy, Age: 10
Name: Pim Pim, Age: 12
Name: Tera-San, Age: 6
```

RAP 是一个相当强大的工具。我们只使用了其功能的一小部分。如果 RDF 是你应用程序的重要组成部分，那么探索这个庞大的库绝对是值得的。

## XMLHttpRequest 对象

我们接下来要查看的技术与我们使用过的面向服务器的工具不同。你可能听说过 AJAX，即异步 JavaScript 和 XML 传输。至少，你可能在 Google Mail 和 Yahoo! Mail 等网站上见过它。AJAX 允许 Web 浏览器在不刷新页面的情况下与服务器交互。与动态 HTML 结合，它在用户和网站之间创造了新的交互水平。在用户面前数据几乎瞬时变化，使 Web 应用程序从未像桌面应用程序那样。

AJAX 的另一个好处是它可以大幅减少 Web 浏览器和 Web 服务器之间的流量。当我们查看传递给 Google Maps 的数据量时，我们就会明白为什么持续刷新会使应用程序变得太慢。

当我们讨论 AJAX 和 XMLHttpRequest 时，我们将构建一个非常简单的 Web 应用程序。该应用程序将从用户获取输入，将其传递给服务器，服务器将 XML 文档发送回浏览器，并使用 JavaScript 动态更改页面。该应用程序的客户端组件在示例代码中为 ajaxTest.html，对应的服务器组件名为 ajaxResponse.php。

不带 JavaScript 代码的 HTML 页面非常基础。

```
<html>
<head>
<script type="text/javascript" language="JavaScript">
...
</script>
</head>
<body>
<form name="theForm" action="#">
    <input type="text" name="inputField" size="10" />
    <input type="button" value="Click Me" />
</form>
<h1>Server Response Area</h1>
<span id="ServerResponse">Nothing yet</span>
</body>
</html>
```

此页面只是一个表单，其下方有一个段落，将使用 JavaScript 更新。

ajaxResponse.php 也同样简单。此脚本将接收名为 field 的查询参数，并将其作为非常简单的 XML 文档传回请求者。

```
<?php
    header("Content-type: text/xml; charset=UTF-8");
?>
<?= '<?xml version="1.0" encoding="utf-8" ?>' ?>
<response>
    <textField>You've entered: <?= htmlentities($_GET['field'])?>
    </textField>
</response>
```

这里的关键是页面将使用名为 field 的查询参数。

### XMLHttpRequest Object Overview

XMLHttpRequest 对象是 AJAX 的核心。这是内置于所有现代 Web 浏览器（5.0 及以上版本）中用于控制 HTTP 请求的对象。此对象类似于内置于 Web 浏览器的其他对象，例如用于控制所有表单元素的 form 对象，或用于控制 Web 浏览器窗口的 window 对象。AJAX 的本质实际上是使用 XMLHttpRequest 在页面加载后由某个 JavaScript 事件触发向服务器发出 HTTP 请求的技术。服务器返回一些数据，XMLHttpRequest 对象将服务器响应传递给页面上的某个 JavaScript 函数。再次使用 JavaScript，页面样式表信息和 Web 浏览器文档对象模型（DOM）会动态更改。让我们逐步了解一个简单的 XMLHttpRequest 的生命周期。

### Using the Object

生命周期由 JavaScript 事件启动。这可以是应用程序所需的任何内容——鼠标悬停、页面加载、按钮点击等。一旦触发，发生的步骤如下：

- 1. 创建 XMLHttpRequest 对象。
- 2. 定义我们将要发出的 HTTP 请求的目标服务器信息（URL、路径、端口等）。
- 3. 就像我们之前使用的 Web 服务一样，我们需要定义要在 HTTP 请求中发送的内容。这可能只是一个空字符串。
- 4. 指定回调函数并构建它。
- 5. 使用对象的 send() 方法发送请求。
- 6. 在回调中捕获服务器响应并使用它来更改页面。

### Creating the Object

创建 XMLHttpRequest 对象有两种方法，具体取决于访客使用的浏览器。如果用户使用的是 Mozilla 浏览器（Firefox、Camino）、Safari 或 Opera，我们只需创建一个新的 XMLHttpRequest() 来创建对象。如果他们使用的是 Internet Explorer 6，我们需要使用 ActiveX 创建一个 Microsoft.XMLHTTP 对象，它是 XMLHttpRequest 的克隆。使用这两种方法将返回的对象放入全局 JavaScript 变量中。我们可以使用 JavaScript 检测 XMLHttpRequest 对象或 Active X 的存在，以确定应使用哪种方法。

```
g_xmlHttp = null;
function createXMLHttpRequest() {
    if (window.XMLHttpRequest) {
        g_xmlHttp = new XMLHttpRequest()
    }
    else if (window.ActiveXObject) {
        g_xmlHttp = new ActiveXObject("Microsoft.XMLHTTP");
    }
}
```

此函数应在请求开始时调用。

### Making the HTTP Request

我们的 HTTP 请求的开始应该在用户执行某些操作之后。我们将在用户在文本字段上释放键时触发请求。也就是说，当用户按下键时，Web 应用程序将调用我们的 JavaScript 函数与服务器通信。

```
<input type="text" name="inputField" size="10" onkeyup="sendRequest()" />
```

此处的函数名为 sendRequest()。现在我们需要编写此函数。此函数将创建 XMLHttpRequest 对象，定义服务器参数，定义在捕获服务器响应时执行的回调函数，然后实际发送请求。

```
function sendRequest() {
    createXMLHttpRequest();
    var url = "/mashups/ch6/examples/ajaxResponse.php?field=" +
        document.theForm.inputField.value;
    g_xmlHttp.onreadystatechange = parseResponse;
    g_xmlHttp.open("GET", url, true);
    g_xmlHttp.send(null);
}
```

此函数中的第一条语句调用 createXMLHttpRequest()，它创建 XMLHttpRequest 对象并将其放入全局变量 g_xmlHttp 中。第二行将服务的 URL 放入变量中。这是服务的虚拟 URL。你也可以使用服务的绝对 URL，但我们稍后会讨论为什么绝对 URL 是不必要的。此语句的最后一部分将我们输入文本框的值放入名为 field 的查询参数中，这正是我们的服务所等待的。
