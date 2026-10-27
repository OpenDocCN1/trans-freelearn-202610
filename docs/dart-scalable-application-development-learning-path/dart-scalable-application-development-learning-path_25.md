## 模块 3

### Dart Cookbook

超过 110 个极其有效、实用且动手实践的方案，用于设计 Dart Web 客户端和服务器应用程序

## 使用 Dart 工具

在本章中，我们将介绍以下内容：

- 配置 Dart 环境
- 设置检查模式和生产模式
- 快速进行 Dart Editor 故障排除
- 托管您自己的私有 pub 镜像
- 将 Sublime Text 2 用作 IDE
- 将应用编译为 JavaScript
- 在 Chrome 中调试 JavaScript 形式的应用程序
- 使用命令行工具
- 解决 `pub get` 失败的问题
- 缩小应用的体积
- 进行系统调用
- 使用快照
- 从操作系统获取信息

### 简介

本章旨在提高我们对 Dart 平台的掌握程度。Dart 是谷歌为现代 Web、Web 客户端以及服务器应用程序推出的新语言。与 JavaScript 相比，Dart 是一种更高级的语言，因此能带来更高的生产力。此外，它还提供了更高的性能。要驾驭这种强大的能力，我们需要一个良好的工作环境，这正是 Dart Editor 所提供的。Dart Editor 本身就是一个相当全面的环境，了解它所展现的更高级和隐藏的功能是值得的。有些功能只能在命令行工具中使用，因此我们也必须讨论这些功能。

### 配置 Dart 环境

本方案将帮助根据我们的需求定制 Dart 环境。在这里，我们配置以下内容：

- 定义 `DART_SDK` 环境变量
- 使 `dart-sdk\bin` 可用于执行 Dart 命令行工具

#### 准备工作

我们假设您的机器上已安装了可工作的 Dart 环境。如果没有，请访问 `https://www.dartlang.org/tools/download.html` 并为您的平台选择 **选项 1**，即完整捆绑包。下载并解压缩后，会生成一个名为 `dart` 的文件夹，其中包含您需要的一切。请将其放入您选择的目录中。这个目录可以是任何位置，但为了方便起见，请保持简短，例如 Windows 上的 `d:\dart` 或 Linux 上的 `~/dart`。在 OS X 上，您可以直接将该目录拖放到 App 文件夹中。

#### 如何操作...

1. 创建一个包含 `dart-sdk` 文件夹路径的 `DART_SDK` 环境变量。在 Windows 上，创建并将 `DART_SDK` 设置为 `d:\dart\dart-sdk`；如果使用其他文件夹中的 dart，则设置为 `<your-dart-sdk-path>\dart-sdk`（如果需要更多关于如何执行此操作的信息，请参阅 `http://www.c-sharpcorner.com/UploadFile/6cde20/use-of-environment-variable-in-windows-8/`）。在 Linux 上，使用 `export DART_SDK=~/dart/dart-sdk` 代码将此添加到您的配置文件 `.bashrc` 和/或 `.profile` 中。在 OS X 上，导出 `DART_SDK=/Applications/dart/dart-sdk`，或一般形式为 `export DART_SDK=/path/to/dart-sdk`。

2. 安装目录有一个子文件夹 `dart-sdk\bin`，其中包含命令行工具。将此子文件夹添加到您的环境路径中。在 Windows 上，将 `%DART_SDK%\bin` 添加到路径（系统环境）变量的前面，然后点击 **确定**。在 Linux 或 OS X 上，将 `export PATH=$PATH:$DART_SDK/bin` 添加到您的配置文件中。

3. 重置您的环境配置文件，或之后重启机器，以使更改生效。

#### 工作原理...

例如，设置 `DART_SDK` 环境变量可以使 dart-maven 等插件能够搜索 Dart SDK。（dart-maven 是一个为 Google Dart 在 maven 构建过程中提供集成的插件。）如果您的操作系统知道 Dart 工具所在的路径，您就可以在终端或命令行会话中的任何位置启动其中的任何工具（例如 Dart VM 或 dartanalyzer）。

通过在终端中输入 `dart` 并按 *Enter* 来测试环境变量。您应该看到以下帮助文本：

**用法：dart [<vm-flags>] <dart-script-file> [<dart-options>]**

**执行作为 <dart-script-file> 传入的 Dart 脚本**

### 设置检查模式和生产模式

在开发或维护期间，应用的执行速度并不那么重要，但有关程序执行的信息却很重要。另一方面，当应用被置于客户环境中运行时，需求几乎相反；速度是最重要的，程序暴露的自身信息越少越好。这就是为什么当应用在 Dart **虚拟机（VM）** 中运行时，它可以有两种运行时模式：

- **检查模式**：也称为调试模式。检查模式在开发期间使用，并给出代码中可能存在的错误的警告和错误信息。
- **生产模式**：也称为发布模式。当您希望应用尽可能快地运行且不受代码检查干扰时，可以在生产模式下部署应用。

#### 准备工作

在 Dart Editor 中打开您的应用，并选择启动网页或 Dart 脚本，通常是 `web\index.html`。





## 使用 Dart 工具

## 如何操作...

+   1. 在 Dart Editor 中工作时，检查模式（checked mode）是默认模式。如果你想使用生产模式，请打开**运行**菜单并选择**管理启动**（*Ctrl* + *Shift* + *M*）。将出现**管理启动**窗口，如下面的截图所示：

![](img/52c05b53a56abd53584e4fed738ed509_554_0.png)

管理启动窗口

+   2. 在**Dartium 设置**下，你会看到**在检查模式下运行**复选框。（如果你选择了 Dart 脚本，它将在**VM 设置**标题下。）取消选中该复选框，即可在生产模式下运行脚本。接下来，点击**应用**，然后点击**关闭**，或直接点击**运行**。此设置将一直保持，直到你再次更改。

使用 `dart` 命令在命令行（或批处理文件）中启动的脚本会在 Dart VM 中运行，因此处于生产模式。如果你想在检查模式下运行 Dart VM，你必须通过以下命令明确指定：

```
dart -c script.dart or: dart --checked script.dart
```

你可以通过从 `dart\chromium` 启动 Chrome 可执行文件来直接启动 Dartium（即带有 Dart VM 的 Chromium）；默认情况下，它以生产模式运行 Dart Editor。如果你想在检查模式下启动 Dartium，可以按照以下步骤操作：

+   - 在 Windows 上，在 `dart\chromium` 文件夹中，点击 chrome 文件
+   - 在 Linux 上，在 `~/dart/chromium` 文件夹中，打开 `./chrome` 文件
+   - 在 OS X 上，打开 `DART_FLAGS` 文件夹，然后打开 `path/Chromium.app`

通过在你刚刚启动的 Chrome 浏览器中访问以下地址来验证此设置：`chromium://version`。

当 Web 应用在 Chrome 中的 Dart VM 中运行时，默认情况下它会以生产模式运行。

## 工作原理...

在检查模式下，类型通过调用形如 `assert (var1 is T)` 的断言来检查，以确保 `var1` 是类型 `T`。每当你执行赋值、向函数传递参数或从函数返回结果时，都会发生这种检查。

然而，Dart 是一种动态语言，类型是可选的。这就是为什么在生产模式下，VM 必须像 `type` 注解（例如 `int n`）不存在一样执行你的代码；这些注解实际上会被丢弃。因此，在运行时，以下语句 `int x = 1` 等同于 `var x = 1`。

创建了绑定 `x`，但没有使用 `type` 注解。

> 避免类型检查使生产模式运行得更快。此外，VM 使用 `type` 推断来生成更快的代码；它观察赋给 x 的值（此处为 1）的类型，并进行相应的优化。

## 扩展内容...

借助检查模式，Dart 可以帮助你在开发期间捕获类型错误。这与 Python、Ruby 和 JavaScript 等其他动态语言形成对比，在这些语言中，错误只能在测试期间被发现，或者更糟糕的是，它们会引发运行时异常。你可以通过从 `main()` 调用 `isCheckedMode()` 函数轻松检查你的 Dart 应用是否在检查模式下运行（参见代码包第 1 章文件夹中的脚本 `test_checked_mode\bin\test_checked_mode.dart`），如下面的代码所示：

```
main() {
    isCheckedMode();
    // your code starts here
}

void isCheckedMode() {
    try {
        int n = '';
        throw new Exception("Checked Mode is disabled!");
    } on TypeError {
```

```
```dart
print("Checked Mode is enabled!");
}
}
```
```

## 下载示例代码

你可以从你的账户 http://www.packtpub.com 下载你所购买的所有 Packt 图书的示例代码文件。如果你在其他地方购买了本书，可以访问 http://www.packtpub.com/support 并注册，以便将文件直接通过电子邮件发送给你。

异常消息将显示在浏览器控制台中。在部署到生产模式之前，请务必删除此调用或将其注释掉；我们不希望在运行时出现异常！

## 参见

+   - 本章的*将应用编译为 JavaScript* 一节，了解如何在应用的 JavaScript 版本中启用检查模式
+   - 本章的*使用命令行工具* 一节，了解其他选项

## 快速排查 Dart Editor 问题

Dart Editor 基于 Eclipse 集成开发环境（IDE），因此它需要 Java VM 才能运行。有时，这可能会引发问题；如果出现这种情况，请务必查阅 Dart 网站上的 Dart Editor 故障排除页面：https://www.dartlang.org/tools/editor/troubleshoot.html。

## 准备工作

Dart Editor 使用的一些 JVM 设置存储在 dart 安装目录的 `DartEditor.ini` 文件中。该文件通常包含以下设置（在 Windows 系统上）：

```
```ini
-data
@user.home\DartEditor
-vmargs
-d64
-Dosgi.requiredJavaVersion=1.6
-Dfile.encoding=UTF-8
-XX:MaxPermSize=128m
-Xms256m
-Xmx2000m
```
```

在 Linux 系统上，`-data` 下方的行将是 `@user.home/.dartEditor`。

## 如何操作...

如果你在编辑器中注意到奇怪或不想要的行为，删除 `settings` 文件夹及其子文件夹（由 `-data` 指向）可以恢复正常。该文件夹根据操作系统的不同位于不同位置；位置如下：

+   - 在 Windows 系统上，`C:\Users\{你的用户名}\DartEditor`
+   - 在 Linux 系统上，`$HOME/.dartEditor`
+   - 在 OS X 系统上，`$HOME/Library/Application Support/DartEditor`

删除设置文件夹不会损害你的系统，因为当你重新打开 Dart Editor 时会立即创建一个新的设置文件夹。不过，你将需要重新加载项目。如果你想保存旧设置，可以重命名该文件夹，而不是直接删除；这样，如果你以后想要恢复旧设置，就可以这样做。

## 工作原理...

`-data` 设置所指向的 Dart Editor 文件夹位于用户主目录中，其中包含编辑器的各种设置（元数据）。清除所有设置会删除编辑器使用的元数据。

## 扩展内容...

**-d64** 或 **-d32** 值指定了 JVM 所需的位宽。你可以通过在终端会话中发出命令 `java -version` 来检查你的安装的这些设置，其输出如下：

```
java version "1.7.0_51"

Java(TM) SE Runtime Environment (build 1.7.0_51-b13)

Java HotSpot(TM) 64-Bit Server VM (build 24.51-b03, mixed mode)
```

如果这与 `-d` 设置不对应，请确保你下载的 Dart Editor 和已安装的 JVM 具有相同的位宽，可以下载与你位宽匹配的 JVM。

> 如果你处理许多 Dart 项目和/或大文件，JVM 的内存消耗将相应增长，你的编辑器将变得非常缓慢且无响应。
> 
> 在 32 位环境中运行几乎会限制你的内存消耗为 1GB，因此如果你看到这种行为，建议切换到 64 位系统（Dart Editor 和 JVM）。然后，你还可以根据已安装的内存大小，将 `-Xmx` 参数的值（默认设置为 2000m = 2 GB）设置为更高的值。这将显著提高你编辑器的加载和工作速度！

如果你的 JVM 未安装在默认位置，你可以在 `.ini` 文件中 `-vmargs` 之前的一行添加以下内容：

```
-vm
  /full/path/to/java
```

如果你遇到问题，可以通过将 Dart SDK 和 Dart Editor 升级到最新版本来解决。在 Dart Editor 菜单中，选择“帮助”，然后选择“关于 Dart Editor”。如果有新版本可用，它会自动下载，完成后，点击“应用更新”。

## 托管你自己的私有 pub 镜像

当 pub 仓库不可达时（因为你没有互联网访问权限，或在非常严格的防火墙后面工作），另一种可能性是托管你自己的私有 pub 镜像。

## 如何操作...

按照以下步骤托管你自己的私有 pub 镜像：

+   1. 你需要一个能与 pub 的 HTTP API 通信的服务器。关于该独立 API 的文档尚不存在，但运行在 pub.dartlang.org 的主 pub 服务器是开源的，其代码位于 https://github.com/dart-lang/pub-dartlang。要在本地运行服务器，请执行以下步骤：
+      1. 安装适用于 Python 的 App Engine SDK。
+      2. 验证其路径在 $PATH 中。
+      3. 安装 pip 安装文件、beautifulsoup4 和 pycrypto webtest 包。
+      4. 从顶层目录运行此命令以启动 pub 服务器 `dev_appserver.py app.`
+      5. 使用 http://localhost:8080/ 在浏览器中验证其是否正常工作。





- 2. 你需要设置一个 PUB_HOSTED_URL 环境变量，使其指向你的镜像服务器的 URL，以便 pub 在那里查找以下载托管的依赖项，例如，PUB_HOSTED_URL = http://me:mypassword@127.0.0.1:8042。
- 3. 手动将你需要的包上传到你的服务器，访问 http://localhost:8080/admin（以管理员身份登录），转到 Private Key 选项卡，并在私钥字段中输入任意字符串。

### How it works...

服务器 https://pub.dartlang.org/ 用 Python 编写，设计用于在 Google App Engine 上运行，但也可以在局域网中运行。

## Using Sublime Text 2 as an IDE

Dart Editor 是一个很棒的环境，但 Sublime Text 也具有许多功能，并且可以与许多其他语言一起使用，使其成为许多开发者的首选编辑器。

### Getting ready

你可以免费下载 Sublime Text 用于评估，然而，为了继续使用，必须从 http://www.sublimetext.com/ 购买许可证。

Google 的 Tim Armstrong 为 Sublime Text 开发了一个 Dart 插件，可以从 GitHub 下载 https://github.com/dart-lang/dart-sublime-bundle，或你可以在本书的代码下载中找到它。最简单开始的方法是先安装 Package Control 插件，按照 https://sublime.wbond.net/installation#st2 的说明进行操作。

### How to do it...

在 Sublime Text 中，按 Ctrl + Shift + P（Windows 或 Linux）或 Cmd + Shift + P（OS X；以下所有命令均适用），点击 Install Package 选择该选项，然后点击并选择 Dart 安装插件。之后你打开的任何 Dart 文件都会显示高亮的语法、匹配的括号等。

另外，点击 Menu Preferences，Settings，然后点击 User 并在该 JSON 文件的第一行添加你的 dart-sdk 路径：

```
{
"dartsdk_path": "path\to\dart-sdk",
...
}
```

如果你想手动安装此插件，将 `dart-sublime-bundle-master` 文件夹的内容复制到 `Sublime packages` 目录下名为 `Dart` 的新目录中。该目录在不同操作系统上的位置不同，分别如下：

- ► 在 Windows 上，这很可能位于 `C:\Users\{your username}\AppData\Roaming\Sublime Text 2\Packages`
- ► 在 Linux 上，这很可能位于 `$HOME/Sublime Text 2/Pristine Packages`
- ► 在 OSX 上，这很可能位于 `~/Library/Application Support/Sublime Text 2/Packages`

### How it works...

该插件有一系列代码片段来方便使用 Dart，例如，输入 `lib` 会展开库语句。其他片段包括 `imp` 用于导入，`class` 用于类模板，`method` 用于方法模板，以及 `main` 用于 main() 函数。在按下 `Ctrl + SHIFT + P` 后在弹出窗口中输入片段，可以查看所有片段的列表。使用 `Ctrl + /` 来（取消）注释选中的代码文本。

该插件还为你创建了一个构建系统。`Ctrl + B` 将调用 dartanalyzer，然后使用 dart2js 编译器将 Dart 代码编译为 JavaScript，如以下屏幕截图所示。编辑并保存 `pubspec.yaml` 文件将自动调用 `pub get` 命令。

![](img/52c05b53a56abd53584e4fed738ed509_560_0.png)

Working in Sublime Text 2

### See also

- Refer to the *Configuring the Dart environment* recipe for the path to the Dart SDK

## Compiling your app to JavaScript

在浏览器中部署 Dart 应用意味着在 JavaScript 引擎中运行它，因此 Dart 代码必须先编译为 JavaScript。这是通过 `dart2js` 工具完成的，该工具本身用 Dart 编写，位于 `dart-sdk` 的 `bin` 子文件夹中。该工具也在 Dart Editor 中得到了很好的集成。

### How to do it...

- 右键单击 `.html` 或 `.dart` 文件并选择 **Run as JavaScript**。
- 或者，你可以右键单击 `pubspec.yaml` 文件并从上下文菜单中选择 **Pub Build**（生成 JS）。你也可以在选择同一文件时点击 **Tools** 菜单，然后点击 **Pub Build**。

### How it works...

第一个选项调用 `pub serve` 命令启动一个本地 Web 服务器，并在其过程中调用 dart2js 进入检查模式。然而，编译后的 `.dart.js` 文件由内部开发 Web 服务器从内存中提供，地址为 `http://127.0.0.1:4031`。这仅适用于开发测试。

在第二个选项中，生成的 files 会写入磁盘中应用的 `build/web` 子文件夹中。这样，你可以将此文件夹复制到生产 Web 服务器并部署你的 Web 应用以在所有现代 Web 浏览器中运行（你只需要部署 `.js` 文件，而不需要 `.precompiled.js` 文件或 `.map` 文件）。然而，Dart Editor 中的 **Pub Build** 默认启用检查模式；请使用控制台中的 `pub build` 命令进行生产模式。

### There's more...

`dart2js` 文件也可以从命令行运行，这是构建非 Web 应用的首选方式。

## Working with Dart Tools

使用 `-o <file>` 或 `-out <file>` 将 dart 脚本编译为输出文件 `prorabbits.js` 的命令是 `dart2js -o prorabbits.js prorabbits.dart`。

如果你想在 JavaScript 版本中启用检查模式，使用 `-c` 或 `-checked` 选项，例如 `dart2js -c -o prorabbits.js prorabbits.dart`。命令 `dart2js -vh` 会给出所有选项的详细概述。

在 `pubspec.yaml` 所在的目录中命令行发出 `pub build` 命令，将与之前选项 2 相同，但还会应用 JavaScript 缩小步骤；以下是 `apptest_pub` 的示例输出：

```
f:\code\test_pub>pub build

Loading source assets... (0.7s)

Building test_pub... (0.3s)

[Info from Dart2JS]:

Compiling test_pub|web/test.dart...

[Info from Dart2JS]:                  Took
0:00:01.770028 to compile test_pub|web/test.dart.   Built 165
files to "build"
```

你可以同时缩小你的应用的 JavaScript 版本和 Dart 版本。

### Producing more readable JavaScript code

要生成更易读的 JavaScript 代码（而不是生产模式的缩小版本，参考 *Shrinking the size of your app* 配方），使用命令 `pub build --mode=debug`，这是 Dart Editor 中的默认命令。

或者，你可以在应用的 `pubspec.yaml` 文件中添加以下 transformers 部分：

```
name: test_pub
description: testing pub

transformers:
  - $dart2js:
      minify: false
      checked: true

dependencies:
  js: any

dev_dependencies:
  unittest: any
```

![](img/52c05b53a56abd53584e4fed738ed509_562_0.png)

> 有关更多信息，请参考 https://www.dartlang.org/tools/pub/dart2js-transformer.html。

### Producing a single Dart file

dart2js 工具也可以用作 Dart 到 Dart 来创建包含应用所需一切的单个 .dart 文件，使用以下命令：

```
dart2js --output-type=dart --minify -oapp.complete.dart app.dart
```

这会获取 Dart 应用，对其进行树摇动、缩小，并生成一个用于部署的单个 .dart 文件。其优点是它会引入依赖项，如第三方库，并对其进行树摇动以消除未使用的部分。

### See also

你可能对本章中的以下配方感兴趣：

- Using the command-line tools
- Shrinking the size of your app
- Debugging your app in JavaScript for Chrome

## Debugging your app in JavaScript for Chrome

在本配方中，我们将研究如何在 Chrome 浏览器中调试你的应用。

### How to do it...

- 1. 从右上角的菜单中，选择 Tools 然后 Developer Tools。
- 2. 通过 Settings（Developer Tools 部分右上角的轮图标）验证 Enable JavaScript source maps 选项是否已打开。确保调试已启用，无论是对所有异常还是仅对未捕获的异常。
- 3. 在 Developer Tools 菜单中选择 Sources，然后按 Ctrl + O 打开文件浏览器并选择你想要调试的 Dart 脚本。

> 在代码行左侧边距点击会放置一个断点，用粗蓝色箭头表示。





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





### 准备工作

Platform 类提供了有关操作系统和应用程序运行的计算机的信息。它位于 dart:io 中，因此我们需要导入此库。

### 如何做...

以下脚本展示了一些有趣选项的使用（请参阅本章的代码文件 `tools\code\platform\bin\platform.dart`）：

```dart
import 'dart:io';

Map env = Platform.environment;

void main() {
  print('We run from this VM: ${Platform.executable}');
  // getting the OS and Dart version:
  print('Our OS is: ${Platform.operatingSystem}');
  print('We are running Dart version: ${Platform.version}');
  if (!Platform.isLinux) {
    print('We are not running on Linux here!');
  }
  // getting the number of processors:
  int noProcs = Platform.numberOfProcessors;
  print('no of processors: $noProcs');
  // getting the value of environment variables from the Map env:
  print('OS = ${env["OS"]}');
  print('HOMEDRIVE = ${env["HOMEDRIVE"]}');
  print('USERNAME = ${env["USERNAME"]}');
  print('PATH = ${env["PATH"]}');
  // getting the path to the executing Dart script:
  var path = Platform.script.path;
  print('We execute at $path');
  // on this OS we use this path separator:
  print('path separator: ${Platform.pathSeparator}');
}
```

运行后，上述代码会给出以下输出：

```
Our OS is: windows
We are running Dart version: 1.3.3 (Wed Apr 16 12:40:55 2014) on "windows_ia32"
We are not running on Linux here!
no of processors: 8
OS = Windows_NT
HOMEDRIVE = C:
USERNAME = CVO
PATH = C:\mongodb\bin;C:\MinGW\bin;...
We execute at /F:/Dartiverse/platform/bin/platform.dart
path separator: \
```

### 它是如何工作的...

大多数选项都很直接。你可以通过 `Platform.executable` 获取正在运行的 VM。你可以通过 `Platform.operatingSystem` 获取操作系统；这也可以通过布尔属性如 `Platform.isLinux` 来测试。Dart 版本可以通过 `Platform.version` 属性进行测试。`Platform.environment` 选项返回系统环境变量的良好映射结构，因此你可以通过名称访问其值，例如，对于变量 `envVar`，使用 `var envVar = Platform.environment["envVar"]`。

要获取正在执行的 Dart 脚本的路径，可以使用 `Platform.script` 的 `path` 属性，因为后者返回脚本的绝对 URI。在应用程序中构建文件路径时，你需要知道路径中各组件的分隔方式；`Platform.pathSeparator` 为你提供此信息。

### 还有更多...

不要将此类与 `dart:html` 中的 `Platform` 混淆，后者返回有关浏览器平台的信息。

## 引言

在本章中，我们主要关注使你的项目更专业并在维护阶段节省大量时间的生命周期中的各种不同任务。这包括结构化项目、在其中安装日志工具、测试、文档化、分析以及发布它。然而，首先我们将看到如何结束一个应用程序，以及服务器应用程序如何接受命令行参数。

### 从应用程序退出

Dart 程序从 Dart VM 中的一个线程（或 isolate）中的 `main()` 函数开始执行。Dart VM 按设计始终以单线程启动。程序可以通过三种不同的方式结束：

- 它可以通过执行 `main()` 中的最后一条语句以正常方式结束，并返回值为 0 的退出代码，表示成功。
- 它可以通过运行时异常异常终止，返回与 0 不同的退出代码，例如未处理的异常情况下为 255。
- 它可以等待事件循环以进行用户交互（如在浏览器中或 Web 服务器等待请求），然后在浏览器关闭或同一浏览器标签中启动另一个应用程序时终止。

然而，我们如何从代码本身退出应用程序？这在某些情况下很有用，例如在服务器端 VM 应用程序中存在可能返回也可能不返回的 Futures。

#### 如何做...

第一个可能性是使用来自 `dart:io` 的顶层函数 `exit(int code)`，如 `exit_app.dart` 中所示，以从无限循环或在特定条件下停止应用程序，并返回退出代码：

```dart
import 'dart:io';

void main() {
  var message = "Dart is fun!";
  int i = 0;
  while (true) {
    print(message);
    i++;
    if (i == 10) {
      print("That's enough!");
      exit(10);
    }
  }
}
```

你也可以使用属性 `exitCode` 设置退出代码值，如以下代码所示：

```dart
exitCode = 10;
// ... other code can be executed
exit(exitCode);
```

#### 它是如何工作的...

`exit(code)` 函数将终止正在运行的 Dart VM 进程并将整数代码作为退出值返回给父进程或操作系统环境，表示程序的成功、失败或其他退出状态。你可以选择代码的值；有一种约定是使用 0 表示成功，1 表示警告，2 表示错误。另一种约定是零表示成功，非零表示失败，并且程序在自然结束时返回警告以表示成功退出。一个具体例子是 dartanalyzer 程序，如果代码生成警告则返回 0。

> 退出代码的含义是特定于平台的；如果你使用 0–127 范围内的退出代码，将不会遇到跨平台问题。

### 解析命令行参数

运行批处理作业的服务器应用程序通常从命令行获取参数值。我们如何在程序中获取这些值？

#### 如何做...

解析命令行参数的明显方法如下（参见 `command_line_arguments.dart`）：

```dart
void main(List<String> args) {
  print("script arguments:");
  for(String arg in args)
    print(arg);
}
```

现在，命令 `dart command_line_arguments.dart param1 param2 param3` 会给出以下输出：

**script arguments:**

**param1**

**param2**

**param3**

然而，你也可以从 Dart Editor 中进行测试，打开菜单 **Run**，并选择 **Manage Launches**（*Ctrl + Shift + M*）。在 **Script arguments** 窗口中填写参数：

![](img/52c05b53a56abd53584e4fed738ed509_578_0.png)

脚本参数

如果你的参数采用 key:value 形式，如以下代码所示？

```
par1:value1 par2:value2 par3:value3
```

在这种情况下，使用以下代码片段：

```dart
for(String arg in args) {
    List<String> par = arg.split(':');
    var key = par[0];
    var value = par[1];
    print('Key is: $key - Value is: $value');
}
```

之前的代码片段会给出以下输出：

**Key is: par1 - Value is: value1**

**Key is: par2 - Value is: value2**

**Key is: par3 - Value is: value3**

`split` 方法返回 `List<String>`，其中包含每个参数的键和值。更复杂的参数解析可以如下进行：

```dart
final parser = new ArgParser();
final argResults = parser.parse(args);
List<String> pars = argResults.rest;
print(pars); // [par1:value1, par2:value2, par3:value3]
```

再次使用 `split` 来获取键和值。

#### 它是如何工作的...

`main()` 函数可以采用可选参数 `List<String> args` 来获取命令行参数。它只需要对参数字符串进行拆分即可获取键和值。

第二个选项使用来自 pub 仓库的 `args` 包，由 Dart 团队编写。在 `pubspec.yaml` 文件的 dependencies 部分包含 `args:any`。然后，你可以通过在脚本顶部包含 `import 'package:args/args.dart';` 来使用该包。`args` 包既可用于客户端也可用于服务器应用程序。它可更具体地用于解析 GNU 和 POSIX 风格选项，并在 https://api.dartlang.org/apidocs/channels/stable/dartdoc-viewer/args/args 上有文档说明。

#### 另请参阅

- 请参阅第 6 章《使用文件和流》中的“在文件中搜索”配方，了解如何使用 args 包配合标志的示例

### 结构化应用程序

所有旨在在生产环境中使用的 Dart 项目都应遵循最佳软件工程实践，因此必须包含特定文件夹结构。结构良好的项目体现了专业性，并让开发人员有熟悉感；在标准化结构中更容易找到方向。此外，如果你想在应用程序中使用一组特定于应用程序的库，这也是必要的，正如我们在下一个配方中将看到的那样。






## 结构化、测试和部署应用程序

### 准备工作

一个旨在独立运行的应用程序，无论是命令行应用程序还是 Web 应用程序，都属于应用程序包；它需要一个 `main()` 入口点。库包将被用作其他应用程序中的依赖项。所有 Dart 项目都依赖于配置文件 `pubspec.yaml`，它描述了应用程序及其依赖项，以及 `pubspec.lock` 文件。这决定了哪些库将被包含在顶层的 `packages` 文件夹中。该文件和 `packages` 文件夹由 pub 工具（更具体地说是 `pub get` 和 `pub upgrade` 命令）生成，不应手动编辑。

### 如何做...

如果您在 Dart Editor 中开发应用程序，在启动新项目时，您需要选择一个项目模板开始，如下表所示：

| 项目类型 | 模板 | 项目文件夹 |
|---|---|---|
| 独立运行的客户端或服务器应用 | 命令行 | `bin` |
| Web 或 Polymer 应用 | Web 应用程序或使用 Polymer 的 Web 应用程序 | `web` |
| Chrome 应用 | Chrome 打包应用 | `web` |
| 库 | 包 | `lib` |

`bin` 文件夹包含一个带有 `main()` 函数的启动脚本。它还可以包含 shell 脚本，例如启动服务器的脚本。在 `web` 文件夹中，您通常会有 `index.html` 和一个 `main.dart` 文件，或者通常是 `app.html` 和 `app.dart` 文件。其他资源文件如 CSS、JavaScript 文件和图片可以放在各自的文件夹 `css`、`js` 和 `images` 中。Polymer 项目通常会包含一个 `web\component` 子文件夹。不要在 `lib` 文件夹中放置带有 `main()` 的脚本。

然后，您希望按如下方式增强项目的结构：

| 文件夹 | 项目类型 | 文件 |
|---|---|---|
| 顶层 | 全部 | `README.md`、`LICENSE`、`CHANGELOG` 和 `AUTHORS` 文件 |
| `doc` | 全部 | `getting_started.md`、`todo.txt` |
| `example` | 全部 | 示例脚本，展示如何使用该应用 |
| `lib` | Web | `src`、`view` 和 `model` 的文件夹 |
| `src` | 服务器 | `src` 和 `model` 的文件夹 |
| `test` | 全部 | 单元测试脚本 |

### 它是如何工作的...

Dart Editor 中的模板为您提供了一个基本结构，但通常您希望添加一些之前指定的文件夹，以提供一个可识别且专业的结构，您可以在其中轻松找到所需的内容，例如项目中包含哪些测试。正如我们在下一个配方中将会看到的，为了使用应用程序库，它们必须放在 `lib` 文件夹中。

### 还有更多...

README 文件（`readme.md`，采用 markdown 语法；请参阅 http://en.wikipedia.org/wiki/Markdown）和 CHANGELOG 文件会显示在 pub 仓库中您包的页面上，因此其内容很重要。

可选文件夹如下：

- `mock`：此文件夹包含用于在测试环境中模拟应用程序某些行为的类，例如使用文本文件代替真实数据库。
- `tool`：此文件夹包含项目所需的工具脚本，如构建脚本、测试运行器等。
- `benchmark`：当性能至关重要时，此文件夹可以包含用于测试性能的示例。

一个同时具有客户端和服务器组件的应用程序的替代结构可以基于之前的结构放置：

- `client`
- `server`
- `core`（或 shared）

Dart 网站上的 `https://www.dartlang.org/tools/pub/package-layout.html` 链接包含一些额外的信息。

### 另请参阅

- 您可能还想阅读本章中的 *发布和部署您的应用* 配方

## 在应用程序中使用库

如前一配方所述，每种类型的应用程序都可以包含一个 `lib` 文件夹，其中至少包含模型类。这些模型类非常重要，因为它们构成了项目的骨干，因此必须在整个应用程序中可访问。您可以通过将它们放在顶层的 `lib` 文件夹中，或更好的是放在 `lib/model` 中来实现。这中心的位置也会使它们突出，并让您代码的其他阅读者更容易找到。

### 如何做...

查看 `bank_terminal` 项目的结构。模型类 `Person` 和 `BankAccount` 放在 `lib\model` 文件夹中。在 `pubspec.yaml` 文件中为您的项目命名：

```
name: bank_terminal
```

然后，使用相同的名称为 `lib` 文件夹中创建的库脚本 `bank_terminal.dart`，其中包含以下代码：

```
library bank_terminal;

import 'dart:convert';

part 'model/bank_account.dart';
part 'model/person.dart';
```

> 库的名称与您的应用名称相同！这对 Dart 脚本本身不是必需的，但这是一种常见且建议的做法。

现在当执行 `pub get` 或 `pub upgrade` 命令时，`packages` 文件夹中会出现一个 `bank_terminal` 文件夹。然后，您可以通过像导入从 pub 下载的任何托管包一样导入它，使该库在其他 Dart 脚本中使用。例如，在 `web\bank_terminal.dart` 中我们有以下代码：

```
import 'package:bank_terminal/bank_terminal.dart';
```

在这种情况下，模型类变得可用。项目结构如下图所示：

![](img/52c05b53a56abd53584e4fed738ed509_583_0.png)

### 它是如何工作的...

pub 工具就是这样设计的，以便于在应用程序中使用内部库。每个在 `lib` 文件夹（或其子文件夹）中声明库的脚本都将被 `pub get` 或 `pub upgrade` 命令拾取。其结果是，带有其所有代码的库被视为一个单独的“内部”包，因此与应用程序依赖的其他包一起放在 `packages` 文件夹中。这使得代码模型更清晰，也使 pub 更容易部署您的代码。

### 还有更多...

pub 工具可以扩展到 `lib` 的几个子文件夹，每个子文件夹包含自己的库文件（例如，位于 model 文件夹中的以 library model 开头的脚本 `model.dart`），然后是一个顶层库文件 `project_name.dart`，其前几行代码如下：

```
library project_name;
import 'model/model.dart';   // 导入 library model
import 'view/view.dart';     // 导入 library view
...
```

这样，来自 `model`、`view` 等的不同库被导入到一个大的库中，然后应用程序按如下方式导入它：

```
import 'package:project_name/project_name.dart';
```

## 使用 assert 对代码进行微测试

为您的应用程序编写测试是必要的，但花费大量时间在琐碎的测试上是不高效的。经常被低估的 Dart 关键字是 `assert`，它可用于测试代码中的条件。

### 如何做...

查看代码文件 `microtest.dart`，其中 `microtest` 是一个如前一配方中所述的内部包：

```
import 'package:microtest/microtest.dart';

void main() {
  Person p1 = new Person("Jim Greenfield", 178, 86.0);
  print('${p1.name} weighs ${p1.weight}' );
  // 许多其他代码和方法调用
  // p1 = null;
  // 再次使用 p1：
  assert(p1 is Person);
  p1.weight = 100.0;
  print('${p1.name} now weighs ${p1.weight}' );
}
```

我们导入 `microtest` 库，其中包含 `Person` 类的定义。在 `main()` 中，我们创建一个 `Person` 对象 `p1`，经过大量代码后，又想再次使用 `p1`，可能在另一个类的不同方法中。我们如何知道 `p1` 仍然引用一个 `Person` 对象？在之前的片段中很明显，但在更复杂的情况下可能更困难。如果 `p1` 例如被解除引用，没有 assert 我们会得到异常 **NoSuchMethodError: method not found: 'weight='**。

然而，如果我们使用 assert 语句，我们会得到一个更清晰的消息：**AssertionError: Failed assertion: line 9 pos 9: 'p1 is Person' is not true.** 你可以通过取消注释行 `p1 = null` 来测试它。

### 它是如何工作的...

`assert` 参数是一个逻辑条件或任何表达式（例如调用返回布尔值的函数），其解析结果为 false 或 true。如果其值为 `false`，则正常执行会通过抛出 `AssertionError` 而停止。

使用 `assert` 测试代码中任何不明显的条件；它可以替代许多简单的单元测试或难以设置的单元测试。可以放心，`assert` 仅在检查模式下工作；它不会影响部署应用程序的性能，因为它在生产模式下会被忽略。

### 还有更多...

使用 `assert` 进行测试在进入方法时测试参数条件（前置条件），以及在离开方法时测试返回值（后置条件）时非常有用。您也可以从 `assert` 中调用一个 `test` 函数（它必须返回布尔值），例如 `assert(testfunction());`。

## 单元测试 Polymer Web 应用

项目应包含一些自动化测试，可以在每次代码更改后运行，以确保之前的功能仍然有效。Dart 的 `unittest` 框架是最佳工具，Dart 网站上有一些优秀的文章可供入门。然而，测试 Polymer Web 应用程序要困难得多，因为 Polymer 的工作方式隐藏了 HTML 在 shadow DOM 中，并且它以异步方式工作，与测试代码无关。

### 准备工作

我们将在 `ClickCounter` 示例中创建一些测试（使用 `polymer` 库的标准 Web 应用程序模板）。您可以在 `polymer1` 应用中找到代码。我们在 `pubspec.yaml` 文件中包含 `unittest` 库，并创建 `test` 文件夹。




### 操作步骤...

在测试文件夹中，我们创建了一个 `test_polymer1.html` 页面，即一个加载测试此功能所需 Polymer 组件的网页。该组件所需的最低内容如下：

```html
<head>
    <title>test_polymer1</title>
    <link rel="import" href="../web/polymer1.html">
    <script type="application/dart" src="test_polymer1.dart"></script>
</head>
```

`test_polymer1.dart` 包含测试脚本：

```dart
import 'package:unittest/unittest.dart';
import 'dart:html';
import 'package:polymer/polymer.dart';
import '../web/clickcounter.dart';

main() {
    initPolymer();
    var _el;
    setUp(() {
        _el = createElement('<click-counter>Click counter test</click-counter>');
        document.body.append(_el);
    });
    tearDown(() {
        _el.remove();
    });
    // tests:
    test('shadowroot elements are created', () {
        expect(querySelector('click-counter').children, isNotNull);
        expect(querySelector('click-counter').shadowRoot.text, isNotNull);
    });
    test('initial text ok', () {
        expect(querySelector('click-counter').shadowRoot.text.contains('click count: 0'), isTrue);
    });
}
```

### 工作原理...

测试网页加载 Polymer 组件，测试脚本加载 unittest 和 polymer 包以及组件类（`clickcounter.dart`）。在 `main()` 中，我们通过 `initPolymer()` 加载 polymer 包；在 `setup()` 中，我们使用辅助方法 `createElement()`，以包含 polymer 标签 `<click-counter>` 的 HTML 字符串作为参数来实例化 Polymer 组件并将其添加到页面。这样做是为了避免默认的 HTML 清理 `createElement()`——它使用空清理器而不是内置清理器（关于此主题的更多信息，请参阅第 5 章“处理 Web 应用”）。然后，我们开始测试，例如：

- `expect(querySelector('click-counter').children, isNotNull);` 确保 Polymer 组件树被创建
- `var button = querySelector('click-counter').shadowRoot.querySelector('#click'); expect(button, isNotNull);` 确保具有 ID `'click'` 的按钮被创建
- `expect(querySelector('click-counter').shadowRoot.text.contains('click count: 0'), isTrue);` 确保初始显示的文本是 `'click count: 0'`。

注意，我们必须深入 Polymer 组件的 `shadowRoot` 才能获取这些信息，如下所示：

- 验证通过 `button.click()` 触发三次按钮点击后，我们的 count 属性的值为 3：

  ```dart
  ClickCounter cc = querySelector('click-counter');
  expect(cc.count, 3);
  ```

### 另请参阅

- 想了解有关 Dart unittest 库的更多信息，请参阅《Learning Dart》一书（Ivo Balbaert、Dzenan Ridjanovic 著，Packt Publishing 出版），或访问 https://www.dartlang.org/articles/writing-unit-tests-for-pub-packages/ 和 https://www.dartlang.org/articles/dart-unit-tests/ 上的优秀文章。这些内容应有助于你进一步了解背景知识，并帮助你在自己的工作中扩展使用 Dart Unittest。

## 为应用添加日志记录

每个生产应用都需要日志记录功能，以便将不同严重级别（信息/警告/调试）的日志消息输出到（Web 浏览器调试）控制台或文件中。本技巧将帮助你快速轻松地实现这一点。

### 准备工作

使用 Dart 团队开发并通过 pub 提供的 logging 包来实现此目的。将其添加到你的 `pubspec.yaml` 文件中，并在代码中添加代码行 `import 'package:logging/logging.dart';`。在 `bank_terminal_polymer` 中可以看到它的实际应用。我们将该导入添加到 Polymer 组件和模型类 `BankAccount` 的代码中。

### 操作步骤...

1. 在 `web\bank_account.dart` 中，顶层代码如下：

   ```dart
   import 'package:logging/logging.dart';
   final Logger log = new Logger('Bank Account');
   ```

2. 我们将构造函数改为以下代码：

   ```dart
   BankAccount.created() : super.created() {
       setupLogger();
       log.info('Bank Account component is created');
   }
   ```

   `setupLogger()` 是你可以定义日志格式的地方；以下代码展示了一种最小的格式：

   ```dart
   setupLogger() {
       // Set up logger.
       Logger.root.level = Level.ALL;
       Logger.root.onRecord.listen((LogRecord rec) {
           print('${rec.level.name}: ${rec.time}: ${rec.message}');
       });
   }
   ```

3. 在 `checkAmount()` 中，我们添加以下警告消息：

   ```dart
   checkAmount(String in_amount) {
       try {
           amount = double.parse(in_amount);
       } on FormatException catch(ex) {
           log.warning("Amount $in_amount is not a double!");
           return false;
       }
       return true;
   }
   ```

4. 在 `lib\bank_account.dart` 文件的模型类中，当 `BankAccount` 对象被创建时，我们添加一条“信息”消息：`log.info('Bank Account is created')`，并在 `transact` 方法中，当余额变为负数时添加一条“严重”消息：

   ```dart
   transact(double amount) {
       balance += amount;
       if (amount < 0 && (-amount) > balance) {
           log.severe("Balance will go negative!");
       }
       date_modified = new DateTime.now();
   }
   ```

5. 然后运行应用，输入金额 `50q`，再输入金额 `-5000`，这将使余额变为负数。这意味着我们将获得以下控制台输出：

   ```text
   INFO: 2014-04-28 11:27:33.525: Bank Account component is created
   FINE: 2014-04-28 11:27:33.551: [Instance of '_Binding']: bindProperties: [value] to [bank-account].[Symbol("bac")]
   FINE: 2014-04-28 11:27:33.557: [bank-account] cancelUnbindAll
   FINE: 2014-04-28 11:27:33.561: [bank-app] cancelUnbindAll
   INFO: 2014-04-28 11:27:33.561: Bank Account is created
   FINE: 2014-04-28 11:27:39.172: >>> [bank-account]: dispatch enter
   INFO: 2014-04-28 11:27:39.176: <<< [bank-account]: dispatch enter
   WARNING: 2014-04-28 11:27:44.089: Amount 50q is not a double!
   SEVERE: 2014-04-28 11:29:02.778: Balance will go negative!
   INFO: 2014-04-28 11:29:02.778: <<< [bank-account]: dispatch transact
   ```

### 工作原理...

Logging 类对象必须首先进行配置，否则不会发生任何操作。这是在 `setupLogger()` 中完成的，它执行以下操作：

- 设置消息的级别（在 SHOUT、SEVERE、WARNING、INFO、CONFIG、FINE、FINER、FINEST、ALL 或 OFF 之间选择，或预定义你自己的级别）。
- 设置事件处理器来监听 `onRecord` 流。这会处理 `LogRecord` 类型的对象，这些对象可以访问名称、时间、消息和堆栈跟踪。然后，你可以编码定义如何处理此事件：打印到控制台、写入文件、发送邮件等。

## 组织、测试和部署应用

这会安装 `dartdoc-viewer` 工具，并为你的文档启动一个本地 Web 服务器，你可以通过 URL `http://localhost:8080` 访问该服务器。以下是 BankAccount 类文档的截图（你可以通过**选项**菜单更改视图）：

![](img/52c05b53a56abd53584e4fed738ed509_592_0.png)

查看文档

### 工作原理...

该命令会在你的应用中创建一个 `docs` 文件夹，并为你的应用的 `lib` 文件夹以及每个导入的包和 Dart SDK 写入包含 API 信息的 JSON 文件。Dartdoc 注释（`/** ... */`）也会被包含在内。

### 更多内容...

若要让文档可供内网 Web 服务器或互联网 Web 服务器使用，请执行以下步骤：

1. 将 `dartdoc-viewer`（编译为 JavaScript）复制到服务器上。
2. 将生成的文件复制到主 URL 下的 `docs` 目录中。

此外，除非使用 `--include-private` 标志，否则私有声明会被忽略。其他各种选项记录在 `https://www.dartlang.org/tools/docgen/#options`。

指定输出文件夹且不为 SDK 和包生成文档的命令如下：

```bash
docgen --out out/doc/api --no-include-sdk --no-include-dependent-packages
--compile lib/app.dart
```

## 分析并基准测试你的应用

Dart 的关键成功因素之一是其性能。Dart VM 内置了优化器以提高执行速度。该优化器需要一定的执行时间才能获得足够的信息来执行其工作。这对于生产环境中的应用来说没有问题，但在测试时，你必须确保满足这一条件。在本技巧中，我们将重点讨论如何更好地对应用程序进行基准测试。

### 准备工作

Dart 团队的 `benchmark_harness` 包正是为此目的而构建的。将其添加到 `pubspec.yaml` 文件中，并在代码中导入它。我们将通过 `template_benchmark` 应用来演示这一点。同时，请确保已解决检查模式检测到的所有问题（因为这些可能会影响执行速度），并确保应用在生产模式下运行（请参阅第 1 章“使用 Dart 工具”中的“设置检查模式和生产模式”技巧）。

### 操作步骤...

执行以下步骤以使基准测试框架正常工作：

1. 导入基准测试库：

   ```dart
   import 'package:benchmark_harness/benchmark_harness.dart';
   ```

2. 定义一个类 `TemplateBenchmark`，包含自己的 `main()` 和 `run()` 方法。它是基准测试库中 `BenchmarkBase` 类的子类。基准测试从 `run()` 开始：

   ```dart
   class TemplateBenchmark extends BenchmarkBase {
     const TemplateBenchmark() : super("Template");
   
     static void main() {
       new TemplateBenchmark().report();
     }
   
     void run() {
       fib(20);
     }
   }
   ```


```
// recursive algorithms:
int fib(int i) {
    if (i < 2) return i;
    return fib(i-1) + fib(i-2);
}
```

```
// int fib(n) => n<2 ? n : fib(n-2) + fib(n-1);
```

```
// iterative algorithm:
//  int fib(int i){
//    int a = 0; int b = 1;
//    for (int n=a; n < b; n++) {
//        a = a + b; b = a;
//    }
//    return a;
// }
```

```
void setup() { }
void teardown() { }
}
```

3. 以下代码启动整个基准测试机制：

```
main() {
    TemplateBenchmark.main();
}
```

4. 如果我们对斐波那契算法的两种递归实现（使用 `if` 和三元运算符）以及迭代算法进行基准测试，会得到以下结果：

```
Template(RunTime): 482.392667631452 us.
Template(RunTime): 498.00796812749 us.
Template(RunTime): 0.2441818187589752 us.
```

正如我们所料，迭代算法的性能比递归实现高出几个数量级。

### 工作原理...

通过扩展 `BenchmarkBase` 类（这里是 `TemplateBenchmark`）创建一个新的基准测试。被基准测试的代码从这个子类的 `run()` 方法中调用。`setup()` 和 `teardown()` 函数代码在基准测试前后运行，用于准备基准测试或在其后进行清理。这些不计入基准测试本身。顶层的 `main()` 函数通过调用 `BenchmarkBase` 子类的 `main()` 来运行基准测试。

### 另请参阅

- 请参阅新的 Observatory 工具来剖析 Dart 应用，网址为 `https://www.dartlang.org/tools/observatory/`

## 发布和部署你的应用

总有一个时刻，你会认为你的应用已经准备好投入生产，并渴望将其交付给客户或用户。如果这是一个 Dart 世界的 Web 应用，那就意味着要编译成 JavaScript。幸运的是，`pub` 工具会处理你应用生命周期的这一阶段，使你的应用能够成功部署。

### 准备工作

这非常简单。要准备，你需要在检查模式和生产模式下运行并测试你的应用。

### 操作步骤...

在命令行或 Dart Editor 中运行 `pub build` 命令（参见*第 1 章：使用 Dart 工具*中的*将应用编译为 JavaScript*方法）。这会在命令行应用或 Web 应用中分别创建带有 `bin` 或 `web` 子文件夹的 `build` 文件夹。`build` 文件夹包含完整的可交付文件。其中生成的文件可以像任何静态内容一样部署。将 JavaScript 文件连同网页和资源一起上传到任何生产 Web 服务器。

### 工作原理...

`pub build` 命令是 Dart 用于创建部署资产的优化命令。它执行 tree shaking，因此只保留执行期间必需的代码，即未调用的函数、类和库会从生成的 `.js` 文件中排除。压缩过程通过将变量、函数等名称替换为更短的名称，并调整代码以减少行数，从而进一步减小文件大小。

### 更多内容...

以下是一些可在云端运行应用的应用程序托管站点：

- Heroku 是面向云托管 Web 应用的平台即服务。它还未正式支持 Dart 运行时，但已经可以用来实现这一点。更多信息请参阅 http://blog.sethladd.com/2012/08/running-dart-in-cloud-with-heroku.html 和 https://www.dartlang.org/codelabs/deploy 上的 Dart 服务器代码实验室。
- 位于 http://www.dartvoid.com/ 的 DartVoid，使用 Vane 中间件框架；它支持 MongoDB。
- 谷歌本身也提供了在 Google Compute Engine 上运行 Dart 服务器的可能性（参阅 http://alexpaluzzi.com/tag/dartlang/），并且未来可以在谷歌的其他云虚拟机上运行。

当你的应用准备好与其他开发者共享时，它也可以发布到 pub 仓库（pub.dartlang.org）。这通过 `pub publish` 命令完成；该命令会验证你的 `pubspec.yaml` 配置文件内容和应用的文件夹结构。更多详细信息，请参阅 https://www.dartlang.org/tools/pub/publishing.html。

## 在检查模式和生产模式中使用不同设置

开发环境和部署环境通常不同。例如，应用在两种环境中可能必须连接到不同的数据库或不同的邮件服务器，以便在开发中使用模拟服务，在生产中使用真实服务，等等。我们如何在两种模式中使用不同的设置，或者实现类似预编译器指令的功能呢？

### 操作步骤...

执行以下步骤以使用不同的设置：

1. 向 `pubspec.yaml` 添加一个 `transformers` 部分，其中包含一个环境行，指定设置、名称和值的映射，如下所示（参见 `dev_prod_settings` 中的代码）：

```
transformers: # or dev_transformers
- $dart2js:
    environment: {PROD: "true", DB: "MongoPROD"}
```

2. 例如，你可以从 `const String.fromEnvironment('DB')` 获取 DB 设置的值，如下列代码所示：

```
import 'dart:html';

void main() {
  print('PROD: ${const String.fromEnvironment('PROD')}');
  bool prod = const String.fromEnvironment('PROD') == 'true';
  if (prod) {
    // do production things
    window.alert("I am in Production!");
    connectDB(const String.fromEnvironment('DB'));
  } else {
    // do developer / test things
    log('In production, I do not exist');
  }
}

void log(String msg) {
  if (const String.fromEnvironment('DEBUG') != null) {
    print('debug: $msg');
  }
}

void connectDB(String con) {
  // open a database connection
}
```

3. 在 Dart VM（以及检查模式）中运行时，控制台输出 `PROD: null`。在 Chrome 中作为 JavaScript 运行时，会弹出警报对话框，并且**开发者工具**中的控制台会显示**PROD: true**。

### 工作原理...

import 初始化器选项需要手动更改代码才能切换环境。transformers 选项使用由编译或运行 Dart 程序的周边系统提供的环境声明。这更好，因为它只需要更改 `pubspec.yaml` 配置文件。然而，目前它仅为 dart2js 定义，以便部署到 JavaScript。

确保 `environment` 缩进；否则，你可能会遇到错误 `"transformers" must have a single key: the transformer identifier.`
