

这种架构的优势在于程序执行期间完全不需要任何锁定或同步。其他虚拟机必须执行此操作，这极大地影响了复杂性和性能。例如，Python 的 **GIL (Global Interpreter Lock)** 已被争论多年，有许多替代方案被提出并进行了实验性实现。

## 多进程 VM

Dart VM 的设计允许多个 isolate 同时运行，这些 isolate 可以在不同的线程、CPU 核心上运行，在 Web 浏览器中则运行在 web worker 中。它们不共享任何资源，例如内存，它们唯一的通信方式是通过消息系统。

## 类设计器

客户想要一个能吸引程序员的功能。他们希望用户能够快速草拟类的想法，包括类名、字段和方法等细节。

## 构建更复杂的对话框

为处理此功能，需要一个自定义对话框——`GenClassDialog` 在 `custom_dialogs.dart` 中。它将继承（扩展）Dialog 类，并自行处理 OK 按钮点击事件。与应用程序中迄今为止的其他对话框不同，此对话框必须将数据返回给文本编辑器：

![](img/52c05b53a56abd53584e4fed738ed509_58_0.png)

输入控件是一个 TextInputElement 和两个 TextAreaElements。占位符属性在现代 Web 浏览器中可用，将用于指导用户输入内容，并且作为奖励，它们是方便的节省空间的方法，因为我们不需要添加额外的元素来标记输入：

```
String className = name.value;
String fieldsSrc = "";
String methodsSrc = "";

List<String> classFields = fields.value.split("\n");
List<String> classMethods = methods.value.split("\n");

classFields.forEach((field) => fieldsSrc += "    var $field;\n");
classMethods.forEach((method) => methodsSrc += "    $method() {};\n");
```

文本从控件中检索并按行拆分为列表。然后使用 forEach 方法遍历列表并构建源代码。

## 构造类

GenClassDialog 的 makeClass 方法中简单类的模板为生成的字段和方法的源代码提供了占位符。此类代码模板被声明为使用三个双引号的多行字符串。Dart 支持字符串插值，这有助于将变量值放入字符串中：

```
result = """  // The $className class.
class $className {

$fieldsSrc

    $className() {}

$methodsSrc
}
""";
```

变量可以嵌入到以 $ 符号为前缀的字符串中。然后在字符串使用时将其替换为变量的实际值。

也可以插入更复杂的代码，例如对象的属性，“The word is ${text.length} characters long”。注意在被求值的表达式周围使用大括号。

## 命令行项目结构

打开 WebStorm，点击 Create New Project，并从 Project Templates 列表中选择 Console application。为你的项目选择一个位置并将其命名为 dart_stats。

应用程序的结构有一个包含 main.dart 的 bin 文件夹，这是应用程序的入口点。main 函数位于此处。项目模板包含其他几项。对于此工具，重点将放在 main 函数上。

> Dart 是跨平台的，并且操作系统平台使用的绝大多数文件系统都是区分大小写的。强烈建议的约定是所有文件名都为小写。

将源代码文件 sourcescan.dart 添加到项目中。它包含用于扫描文本并统计类、代码、注释、导入和空白行的代码。

## 处理源代码

SourceCodeScanner 类包含五个整数字段来存储计数和一个执行分析的单一方法：

```
void scan(List<String> lines) {
    totalLines = lines.length;
    lines.forEach((line) {
        line = line.trim();
        if (line.startsWith("class")) classes++;
        else if (line.startsWith("import")) imports++;
        else if (line.startsWith("//")) comments++;
        else if (line.length == 0) whitespace++;
    });
}
```

遍历行列表并使用 trim 方法删除行中的多余空白，以帮助匹配触发并增加计数的关键字。startsWith 方法确保关键字不会在另一上下文中出现在行中间。

### 使用 dart:io 包进行文件处理

应用程序将接收一个命令行参数，即 Dart 源代码文件的完整文件路径。命令行参数可以从 main.dart 中 main 函数传递的参数中读取。

我们不必在浏览器中处理的一个方面是从文本文件加载源代码。由于安全原因，不可用于 Dart Web 浏览器的一个包是 io 包，它包含直接的文件处理功能：

```dart
import 'sourcescan.dart';
import 'dart:io';

main(List<String> arguments) {
  print(arguments[0]);
  SourceCodeScanner codeScan = new SourceCodeScanner();
  File myFile = new File(arguments[0]);
  myFile.readAsLines().then((List<String> lines) {
    codeScan.scan(lines);
    print("${codeScan.totalLines}");
    print("${codeScan.classes}");
    print("${codeScan.comments}");
    print("${codeScan.imports}");
    print("${codeScan.whitespace}");
  });
}
```

一旦文本文件在字符串中，它就可以像在 Web 文本编辑器中一样进行处理。

程序可以从命令行运行，如下所示在 Linux 上：

```text
$ dart bin/main.dart   lib/dartstats.dart
lib/dartstats.dart
9
0
3
0
2
```

程序可以检查它自己的源代码！

有关更高级的命令行参数处理，请参阅 pub 上的非常强大的 args 包 https://pub.dartlang.org/packages/args。

readAsLines 是一个异步函数，立即返回一个 Future 对象。函数被传递到 future 对象的 then 方法中。此函数的执行时间对开发者来说是未知的。程序的流程会立即继续。当文件读取操作运行并完成后，传递给 then 方法的函数会在 Dart VM 调度时执行。

要亲眼看到这一点，将 print 语句移出 then 函数，如下所示，然后再次运行程序：

```
myFile.readAsLines().then((List<String> lines) {
    codeScan.scan(lines);
});
print("${codeScan.totalLines}");
print("${codeScan.classes}");
print("${codeScan.comments}");
print("${codeScan.imports}");
print("${codeScan.whitespace}");
```

可能的输出全部为零，因为代码行尚未被扫描；这是因为 print 函数被首先调用。

程序也可能像之前一样运行。关键点是整体程序执行会继续前进，即使代码被要求执行可能耗时的任务，也允许其他任务继续并保持应用程序响应。

## 调试命令行程序

如果你在 WebStorm 中使用 Run 启动程序，它将打开调试器并出现 RangeError 异常。这是因为程序假设已传递命令行参数，而程序是在没有任何输入的情况下运行的。

## 推进编辑器

在 WebStorm 中，可以配置程序的调试启动，包括设置命令行参数的功能。在这种情况下，应在 **Program arguments** 字段中输入 Dart 源文件的完整文件路径。请注意，默认情况下不会传递任何命令行参数。也可以设置多个具有不同命令行选项集的启动：

![](img/52c05b53a56abd53584e4fed738ed509_64_0.png)

要配置启动，请从 **Run** 菜单中选择 **Edit Configurations** 选项。从列表中选择 `main.dart` 并输入任何 Dart 源代码文件的完整文件路径，例如本书的示例代码或来自 Dart SDK 的代码。

一旦程序运行，结果将使用 `print` 函数打印到标准输出。
