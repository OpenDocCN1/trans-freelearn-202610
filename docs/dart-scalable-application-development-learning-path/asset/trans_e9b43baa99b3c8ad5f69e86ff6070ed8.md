

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

