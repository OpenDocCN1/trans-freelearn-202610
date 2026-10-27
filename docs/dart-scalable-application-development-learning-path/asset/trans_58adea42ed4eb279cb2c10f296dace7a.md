
## 使用文件和流

## 如何实现...

使用以下方法检查命令行项目 processing_lines 的代码：

- 1. 使用 readAsLines 方法，如下面的代码所示：

```dart
import 'dart:io';
// for step 3:
import 'dart:async';
import 'dart:convert';

main() {
  File data= new File("../winequality-red.csv");
  data.readAsLines().then(processLines)
    .catchError((e) => handleError(e));
}

processLines(List<String> lines) {
  // process lines:
  for (var line in lines) {
    print(line);
  }
}

handleError(e) {
  print("An error $e occurred");
}
```

上述代码输出如下：

```text
"fixed acidity","volatile acidity","citric acid","residual sugar","chlorides","free sulfur dioxide","total sulfur dioxide","
7.4;0.7;0;1.9;0.076;11;24;0.9978;3.51;0.56;9.4;5: 48 bytes
7.8;0.88;0;2.6;0.098;25;67;0.9968;3.2;0.68;9.8;5: 48 bytes
7.8;0.76;0.04;2.3;0.092;15;54;0.997;3.26;0.65;9.8;5: 51 bytes
11.2;0.28;0.56;1.9;0.075;17;60;0.998;3.16;0.58;9.8;6: 52 bytes
7.4;0.7;0;1.9;0.076;11;34;0.9978;3.51;0.56;9.4;5: 48 bytes
7.4;0.66;0;1.8;0.075;13;40;0.9978;3.51;0.56;9.4;5: 49 bytes
7.9;0.6;0.06;1.6;0.069;15;59;0.9964;3.3;0.46;9.4;5: 50 bytes
7.3;0.65;0;1.2;0.065;15;21;0.9946;3.39;0.47;10;7: 48 bytes
7.8;0.58;0.02;2;0.073;9;18;0.9968;3.36;0.57;9.5;7: 49 bytes
```

- 2. 将每行数据提取到一个对象中，如下面的代码所示：

```dart
processLines(List<String> lines) {
  // process lines:
  for (var line in lines) {
    print(line);
    // when not header line, split line on separator:
    if (!header) {
List<String> fields = line.split(",");
Wine wn = new Wine();
wn.fixed_acidity = fields[0];
wn.volatile_acidity = fields[1];
// extracting remaining properties
wn.alcohol = fields[10];
wn.quality = fields[11];
print(wn);
}
header = false;
}
}
class Wine {
    var fixed_acidity;
    var volatile_acidity;
    // other properties
    var alcohol;
    var quality;

    toString() => "This wine has $fixed_acidity fixed acidity,"
    "alcohol % of $alcohol and quality $quality.";
}
```

上述代码给出的输出是 **这款葡萄酒的固定酸度为 6.8，酒精含量为 11.3%，质量为 6**。

- 3. 使用 openRead 方法作为替代方案，如下面的代码所示：

```dart
main() {
  File data= new File("../winequality-red.csv");
  // using openRead:
  Stream<List<int>> input = data.openRead();
  input
  .transform(UTF8.decoder) // Decode to UTF8.
  // Convert stream to individual lines.
  .transform(const LineSplitter())
  .listen((String line) { // Callback to process results.
    print('$line: ${line.length} bytes');
    // Further processing of line, e.g. as in processLines
  }, onDone: () {
    print('File is now closed.');
  }, onError: (e) {
    print(e.toString());
  });
}
```

```dart
print(data);
// [228, 189, 160, 229, 165, 189, 228, 184, 150, 231, 149, 140]
print(UTF8.decode(data)); // prints '你好世界'.
});
```

- 4. 我们可以使用 openWrite 写入文件，如下面的代码所示：

```dart
var file = new File('out3.txt');
var sink = file.openWrite();
sink.write('File was written to at ${new DateTime.now()}\n');
// close the IOSink to free system resources!
sink.close();
}
```

## 工作原理...

第 1 步使用异步的 writeAsString 方法将一个（大的）字符串写入文件，然后该文件会自动关闭。在由 then 调用的回调函数中，例如，你可以通过网络发送该文件。第 2 步展示了如何使用 writeAsBytes 将原始字节写入文件。当文件包含不可读字符或 Unicode 字符时，这是必需的。

## 更多内容...

如果我们想要分块写入文件，该怎么办？这时，我们使用第 3 步中所示的 openWrite 方法。当在 File 对象上调用此方法时，它会为该文件创建一个 IOSink 对象，你可以使用以下任意方法向其写入：write、writeln、writeCharCode、writeAll。与前几步的 write 方法相比，IOSink 对象在不再需要时必须显式关闭。openWrite 方法接受两个可选参数，如下面的代码所示：

```dart
file.openWrite(mode: FileMode.APPEND, encoding: ASCII);
```

默认模式是 FileMode.WRITE。

## 在文件中搜索

在本节中，我们演示如何搜索文本文件中的特定词语。你可以在项目 searching_file 的 search.dart 脚本中找到相关代码。我们使用 taoprog.txt 作为示例文本文件。

-n 标志是可选的；如果包含该标志，我们会看到行号打印在该行的前面。

以下是脚本中的代码：

```dart
import 'dart:io';
import 'package:args/args.dart';

const HOWTOUSE = 'usage: dart search.dart [-n] search-pattern file';
const LINENO = 'line-number';
ArgResults argResults;
var searchTerms = "";
File file;
```

执行以下步骤在文件中搜索：

- 1. 我们可以使用 args 包搜索文件，如下面的代码所示：

```dart
void main(List<String> args) {
  final parser = new ArgParser()..addFlag(LINENO, negatable:
  false, abbr: 'n');
  argResults = parser.parse(args);
  if (argResults.rest.length < 2) {
    print(HOWTOUSE);
    exit(1);
  }
}
```

- 2. 我们可以通过获取文件名和搜索词来搜索文件，如下面的代码所示：

```dart
final strFile = argResults.rest.last;
File file = new File(strFile);
searchTerms = argResults.rest.sublist(0,
argResults.rest.length - 1);
searchFile(file, searchTerms);
}
```

- 3. 我们可以通过读取文件并搜索来搜索文件，如下面的代码所示：

```dart
searchFile(File file, searchTerms) {
  file.readAsLines().then(searchLines).catchError(print);
}

searchLines(lines) {
  for (var i = 0; i < lines.length; i++) {
    for (var j = 0; j < searchTerms.length; j++) {
      if (lines[i].contains(searchTerms[j])) {
        printMatch(lines[i], i);
      }
    }
  }
}
```

- 4. 我们可以通过打印找到的匹配行来搜索文件，如下面的代码所示：

```dart
void printMatch(String line, int i) {
    StringBuffer sb = new StringBuffer();
    if (argResults[LINENO]) sb.write('${i + 1}: ');
    sb.write(line);
    print(sb.toString());
}
```

## 工作原理...

在第 1 步中，我们看到使用 args 包解析命令行参数。选项 -n 要么存在，要么不存在（开或关，是一个布尔值），这就是为什么它作为标志添加到解析器对象中；negatable:false 防止你将 no-n 作为参数写入。我们进行解析，然后检查是否至少有两个参数（一个搜索词和一个文件名）。如果没有，则显示常量 HOWTOUSE 中的字符串作为帮助选项。第 2 步准备场景；最后一个参数是文件名，由 sublist 方法给出的其余参数是搜索词列表。

实际的搜索发生在第 3 步；我们使用 readAsLines 方法读取文件。当读取完成后，回调函数 searchLines 在嵌套的 for 循环中被调用，其中每一行依次针对所有搜索词进行测试，看它是否包含该词。因此 printMatch 会打印该行，并且如果指定了 -n，还会在该行前面打印行号。

## 另请参阅

- 有关使用 args 包的更多信息，请参阅第 2 章“构建、测试和部署应用程序”中的“解析命令行参数”一节，网址为 https://pub.dartlang.org/packages/args
- 你可以在 https://code.google.com/p/dart/source/browse/branches/bleeding_edge/dart/samples/dgrep/bin/dgrep.dart 找到一个递归搜索文件夹结构的示例。

## 拼接文件

假设我们有一些文本文件，想要将它们拼接成一个大文件。本节中的代码位于项目 concat_files 中，展示了如何实现这一点。

## 如何实现...

该程序在 `bin` 文件夹中从命令行启动（或者在 Dart Editor 中使用 Managed Launch 并设置脚本参数 `file1.txt file2.txt file.txt`），命令为 `dart concat.dart file1.txt file2.txt file.txt`，其中 `file1.txt` 和 `file2.txt` 是要拼接（可以有两个或更多文件）到 `file.txt` 中的文件。以下是执行此操作的代码：

```dart
import 'dart:io';
import 'package:args/args.dart';

ArgResults argResults;
File output;

void main(List<String> arguments) {
  final parser = new ArgParser();
  argResults = parser.parse(arguments);
  final outFile = argResults.rest.last;
  List<String> files = argResults.rest.sublist(0, argResults.rest.length - 1);
  if (files.isEmpty) {
    print('No files provided to concatenate!');
    exit(1);
  }
  output = new File(outFile);
  if (output.existsSync()) {
    output.delete();
  }
  concat(files);
}

concat(List<String> files) {
  for (var file in files) {
    var input = new File(file);
    try {
      var content = input.readAsStringSync();
      content += "\n";
      output.writeAsStringSync(content, mode: FileMode.APPEND);
    } catch (e) {
      print("An error $e occurred");
    }
  }
}
```

