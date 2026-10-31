
# 已知问题

*   在编译 GUI 控制台时，一些杀毒软件（如 Avast 或 Avira）会报告 Win32:Malware-gen 或类似内容。这是因为它们的启发式搜索机制将 Red 编译的二进制文件误报为假阳性。解决方法是将 `C:\\ProgramData\\Red` 文件夹以及其他编译 Red 可执行文件的文件夹添加到杀毒软件界面的“排除文件夹”列表中，使其被列入白名单且不被杀毒程序检查。

*   当从安装文件夹以外的文件夹在控制台中启动 Red 时，你可能会看到以下错误——`PROGRAM ERROR: Invalid encapsulated data`。这是一个已知问题（[https://github.com/red/red/issues/543](https://github.com/red/red/issues/543)），目前是由 REBOL 支持引起的。目前运行 Red 需要一个包装的 shell 脚本 `red.bat`：
  *   将 `red.exe` 重命名为 `red-exe.exe`
  *   在同一文件夹中，创建一个 `red.bat` 文件，其内容为：`@"%~dp0red-exe.exe" %\*`
  *   现在，执行 `red` 命令将自动启动此 `red.bat` 命令文件

# 在 Linux 和 macOS 上安装 Red

要在 Linux 或 macOS 系统上安装 Red，你需要按照以下步骤操作：

1.  从 [http://www.red-lang.org/p/download.html](http://www.red-lang.org/p/download.html) 保存文件到你选择的某个文件夹，例如 `/home/user/red`（其中 `user` 是你的用户名）。
2.  将文件 `red-num` 重命名为 `red`。
3.  在你的终端中，进入 `red` 子文件夹，并执行 `chmod u+x red` 为 Red 二进制文件授予用户执行权限。
4.  如果你在 64 位 Linux 系统上工作，请安装一些 32 位支持库（这是为了执行 REBOL 2 所需的库）。打开一个终端应用程序并输入以下命令：
```
$ sudo dpkg --add-architecture i386
$ sudo apt-get update
$ sudo apt-get install libc6:i386 libcurl3:i386
```
5.  现在只需在终端窗口中使用 `red` 命令运行 Red 即可。

Red 控制台只会在第一次构建。控制台可执行文件构建在 `/home/user/.red` 文件夹内，名称格式为 `console-yyyy-m-dd-nnnnn`。

之后，命令行控制台会打开：

![](assets/64983179-6fdb-47e4-a414-9ce648561387.png)

要在整个文件系统中都能使用 Red，请在你的 `.profile` 和 `.bashrc` 脚本中添加以下行：
```
export PATH=$PATH:/home/user/red
```
（在前面的命令中，`user` 是你的用户名。）

然后，打开一个新的终端应用程序。现在可以从任何目录启动 Red 可执行文件。

如果你像我一样在 `/bin` 中有一个 Red Linux 实用脚本，你必须按如下方式重命名它：
`cd /bin`
`sudo mv red red_old`

如果你遇到无效压缩数据问题，这与 Windows 中提到的 REBOL 问题相同。可以通过创建 shell 脚本 `red.sh` 并使用以下内容来解决：
```
#!/bin/bash
red $@
```
不要忘记使用 `chmod u+x red.sh` 使 shell 脚本可执行。

在 Linux 中，Red 命令行版本的替代方案是安装 WineHQ ([https://www.winehq.org/](https://www.winehq.org/))，这是一个可在 Linux 上运行 Windows 程序的工具。然后你可以使用图形控制台运行 Red Windows 版本。

对于 Arch Linux、FreeBSD 和其他特定发行版，你可以在下载页面找到进一步的说明。

Red 也可以通过使用 Chocolatey 包管理器 ([https://chocolatey.org/packages/red](https://chocolatey.org/packages/red)) 安装。

无论你使用哪个操作系统，要快速查看 Red 安装的版本号，请打开终端并输入：
```
red -V
```
这将给出版本号 `n.u.m`（例如，`0.6.3`）。

# 查看源代码

Red 的源代码可以在这里找到：[https://github.com/red/red](https://github.com/red/red)。如果你感兴趣，可以在本地下载一个归档文件（点击 Clone or download 按钮）并解压以便更仔细地查看，例如在你的 Red 安装目录内。或者，如果你的机器上安装了 `git`，你可以使用以下命令克隆源代码：
```
git clone https://github.com/red/red.git
```
为了便于参考，将源文件夹重命名为 `red-source`。以下是概览：

![](assets/fa24ee2f-cdd4-4843-9d1a-5bcda1e2979b.png)

你会注意到很多源文件都有 `.r` 扩展名，这意味着它们是 REBOL 文件。事实上，在这个阶段，Red 框架是用 REBOL 搭建的。一些重要文件是：
*   `red.r`: 这是 Red 和 Red/System 编译器
*   `run-all.r`: 这是运行完整的 Red 测试套件的起点

Red 团队的目标是用 Red 完全重写此脚手架，从而移除对 REBOL 的依赖。这可能在 v2 中发生。然后，Red 将完全自托管，每个新版本都将从旧的 Red 版本启动。

在同一层级的文件夹中，你可以创建一个 `projects` 文件夹，在其中开始开发：
*   `red`
  *   `red-source`
  *   `projects`
  *   `red` 二进制文件或 shell 脚本

# 在 Red 控制台中交互工作

像任何动态语言一样，Red 有一个 REPL 控制台。不带参数的 `red` 命令（在 Windows 和 macOS 上打开或双击 `red.bat`）在 Windows 和 macOS 上启动图形窗口控制台，在 Linux 上启动命令行控制台。如果你需要在 Windows 和 macOS 上使用命令行控制台，只需使用命令 `red --cli` 即可。

控制台是一个完整的 Red 解释器，并在 `\>>` 提示符后期望输入 Red 代码，以便在按下 `Enter` 时进行求值然后执行。它是与语言的实时交互。简单地给出一个值（这里是一个用 `""` 表示的字符串）会在 `\==` 之后返回该值：
```
;-- see Chapter02/console-examples.red:
>> "Red is awesome"
== "Red is awesome"
```
（你可以在 `ch2/console-examples.red` 中找到这些示例的源代码。在所有代码文件中，我们将使用 `;==` 前缀来表示返回的输出，像这样，`;== "Red is awesome"`）。

如果在控制台中输入 `Red is awesome` 会怎样？REPL 会向你抛出错误——`Script Error: is does not allow word! for its 'field argument`。Red 看不到输入是一个字符串，所以无法理解它。

继续做一些计算：

![](assets/6f0e1e2a-2711-4e8b-8cb7-367b9b9eed56.png)

在第二行，我们看到了第一个语法错误。这意味着你必须在二元运算符（如 `\*`）前后使用空格。请注意，使用整数的 `/` 返回整数结果。

控制台有一个内置的帮助和文档系统，你可以通过输入 `?` 或 `help` 来启动：

![](assets/4d5f38a2-cb1c-414b-bf0f-b169930c43ef.png)

尝试示例以感受其搜索和补全功能（单词 **模塑的** 的意思是：在可读的格式中）。

以下是有关在控制台中工作的一些其他有用提示：
*   上箭头和下箭头用于检索上一个和下一个控制台命令，然后可以对其进行修改和/或重新执行。事实上，控制台命令的历史记录存储在 `system/console/history` 块中，并且从版本 0.6.4 开始，这在会话之间得以保留。在 [第6章](#0f4412a2-a1fb-4241-a1cf-98fed354b645.xhtml) 中，*使用文件，* 在 *加载和保存文件* 部分，我们将学习一个不错的技巧来将控制台命令的历史记录保存到文件中以便更轻松地处理。
*   要清除屏幕，请使用 `CTRL+L`。
*   你可以随时通过关闭窗口图标关闭控制台，或从提示符处输入 `q` 或 `quit` + `Enter`（在 Linux 上，也可使用 `CTRL+D`）关闭。
*   `Home` 和 `End` 将光标定位在行的开头和结尾。
*   在 GUI 控制台中，当你想从（正在运行的）语句返回到提示符时，使用 `Esc` 键；对于命令行界面使用 `Ctrl+C`。
*   使用鼠标或常用的键盘快捷键（`Ctrl+C` - `Ctrl+V`）复制/粘贴代码（甚至整个 Red 脚本）。
