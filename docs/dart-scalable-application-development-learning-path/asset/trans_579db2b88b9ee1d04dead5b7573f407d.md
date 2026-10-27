

## 在 Firefox 中调试 JavaScript 应用

在 Firefox 中，源映射功能尚未实现。使用 Shift + F2 打开开发者工具栏和命令行。在顶部菜单中，你会看到 Debugger。设置断点并重新加载文件。代码执行随后会停止，你可以检查变量的值，如以下屏幕截图所示：

![](img/52c05b53a56abd53584e4fed738ed509_565_0.png)

## 使用命令行工具

有些事情在命令行上完成起来更简单，或者只是尚未包含在 Dart Editor 中。这些工具位于 dart-sdk/bin 中。它们包括以下内容：

- dart：用于运行 Dart 命令行应用的独立 Dart VM，例如服务器端脚本和服务器应用
- dartanalyzer：用于静态检查代码
- pub：这是一个包和仓库管理器
- dartfmt：这是一个代码格式化工具
- docgen：这是一个文档生成工具

### 如何操作...

1. 对于每个工具，了解或检查其版本可能很有用。这是通过 `--version` 选项完成的，例如 `dart --version`，典型输出为 **Dart VM version: 1.3.0 (Tue Apr 08 09:06:23 2014) on "windows_ia32"**。
2. `dart -v -h` 选项列出并讨论 VM 的所有可能选项。许多工具还接受 `--package_root=<path>` 或 `-p=<path>` 选项来指示导入中使用的包在文件系统上的位置。
3. `dartanalyzer` 用 Java 编写，在项目导入或 Dart 代码更改时在 Dart Editor 中工作；它以 `dartanalyzer prorabits.dart` 启动，输出为：

   **Analyzing prorabits.dart...**

   **No issues found (or possibly errors and hints to improve the code)**

4. 上述输出验证代码符合语言规范 `https://www.dartlang.org/docs/spec/`，pub 功能已内置于 Dart Editor，但该工具也可以从命令行使用（请参阅 `test_pub`）。要获取包（例如 `test_pub` 应用），在 `pubspec.yaml` 所在的文件夹中使用以下命令，`pub get`，典型输出如下：

   `Resolving dependencies... (6.6s)`
   `Got dependencies!`

5. 会创建一个 packages 文件夹，其中包含指向机器上中央 `package` 缓存的符号链接。最新版本将被下载，并且包版本会注册在 `pubspec.lock` 文件中，以便你的应用只能使用这些版本。
6. 如果你想获取包的新版本，请使用 `pub upgrade` 命令。你可以使用 `-v` 和 `--trace` 选项生成详细输出以验证其工作情况。
7. `dartfmt` 工具也内置于 Dart Editor。右键单击任何 Dart 文件并从上下文菜单中选择 **Format**。这会将代码转换以使其符合 Dart 风格指南，可在 `https://www.dartlang.org/articles/style-guide/` 查看。你也可以从命令行使用它，但此时默认操作模式是清理空白。使用 `-t` 选项应用代码转换，例如 `dartfmt -t -w bank_terminal.dart`。

> 开始处理项目时，如果项目已包含包版本，请始终执行 pub upgrade！

### 另请参阅

- Solving problems when `pub get` fails
- Compiling your app to JavaScript (for `pub build`)
- Documenting your code from Chapter 2, Structuring, testing, and deploying an application
- Publishing your app to a pub (for pub publishing)
- Using snapshotting to start an app in Dart VM
- For additional information, refer to https://www.dartlang.org/tools/

## Solving problems when pub get fails

pub 包管理器是一个功能复杂的工具，因此偶尔出现问题并不奇怪。`pub get` 命令会下载应用所需的所有库，如 `pubspec.yaml` 文件中指定的那样。在代理或防火墙后面运行 `pub get` 曾经是个问题，但在大多数情况下已得到解决。如果这仍然困扰你，请查看 https://www.dartlang.org/tools/editor/troubleshoot.html 中的相应部分。

### Getting ready

当你在 Dart Editor 中尝试打开项目并在 pub get 阶段时，在 Dart 控制台中遇到以下错误时，本食谱尤其有用：

```
Pub install fails with 'Deletion failed'
```

### 如何操作...

首先尝试这样操作；右键单击你的项目并选择 **Close Folder**。然后，重新启动编辑器并再次打开你的项目。在许多情况下，你的项目将正常加载。如果这不起作用，请尝试 `pub get` 命令：

1. 从 `C:\Users\{your username}\AppData\Roaming\Pub.` 删除 pub 缓存文件夹
2. 删除项目中的所有 `packages` 文件夹（也在子文件夹中）。
3. 删除项目中的 `pubspec.lock` 文件。
4. 再次从命令行运行 `pub get` 或在 Dart Editor 菜单中选择 **Tools**，然后选择 **Pub Get。**

### 如何工作...

`dart2js` 文件利用了 tree-shaking 功能；只有执行期间必要的代码会被保留，即未被调用的函数、类和库会被排除在生成的 `.js` 文件之外。压缩过程进一步通过用更短的名称替换变量、函数等名称并移动代码以使用几行来减小大小。

### There's more...

使用反射时要小心。

### More Information Section 1

在 Dart 代码中使用反射会阻止 tree shaking。因此，只有在确实需要时才导入 `dart:mirrors` 库。在这种情况下，包含 `@MirrorsUsed` 注解，如以下代码所示：

```
library mylib;

@MirrorsUsed(targets: 'mylib')
import 'dart:mirrors';
```

在前面的代码中，`mylib` 内部的所有名称和实体（类、函数等）都将在生成的代码中保留以使用反射。因此，创建一个单独的库来保存使用 mirrors 的类。

> 确保你的部署 Web 服务器使用 `gzipping` 进行实时 HTTP 压缩。

### 另请参阅

- You might want to consult the *Using Reflection* recipe in *Chapter 4, Object Orientation.*

## Making a system call

一个相当常见的用例是你需要从 Dart 应用调用另一个程序或操作系统命令。为此，`dart:io` 包中创建了抽象类 `Process`。

### 如何操作...

要从 Dart 脚本 `prorabits.dart` 生成名为 `prorabits` 的脚本快照文件，请发出以下命令：

```
dart --snapshot=prorabits prorabits.dart
```

然后，使用 `dart prorabits args` 启动应用，其中 args 代表脚本所需的可选参数。

### 如何工作...

脚本快照是应用对象在内存中的字节表示（更准确地说是在启动的 isolate 的堆中），在加载后但在开始执行之前。这使得启动速度更快，因为对应用代码进行标记化和解析的工作已在快照中完成。

### There's more...

本食谱适用于服务器应用或命令行应用。内置 Dart VM 的浏览器可以自动为你的 Web 应用创建快照并将其存储在浏览器缓存中；下次请求该应用时，它会从快照中更快地启动。因为快照实际上是对象(s)的序列化形式，所以这也是 Dart VM 在 isolate 之间传递对象的方式。文件夹 `dart/dart-sdk/bin/snapshots` 包含主要 Dart 工具的快照。

### 另请参阅

- Occasionally, your app needs access to the operating system, for example, to get the value of an environment variable to know where you are in the filesystem, or to get the number of processors when working with isolates. Refer to the *Using isolates in the Dart VM* and *Using isolates in web apps* recipes, in *Chapter 8, Working with Futures, Tasks, and Isolates*, for more information on working with isolates.

## Getting information from the operating system

在本食谱中，你将看到如何通过进行系统调用并从系统获取信息来与应用运行的底层操作系统进行交互。

