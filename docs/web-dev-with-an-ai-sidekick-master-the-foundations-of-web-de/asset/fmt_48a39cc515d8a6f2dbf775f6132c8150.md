
在 Django 项目中，你经常运行：

```
python manage.py runserver
```

你可以将其缩短为单个单词：

```
alias serve="python manage.py runserver"
```

现在你可以通过输入以下内容启动开发服务器：

```
serve
```

这与完整命令的作用相同，但输入更快且更易记忆。

当你输入命令时，Bash 首先检查：

1. 这是别名吗？
2. 如果是，则用完整命令替换它
3. 然后执行结果

因此：

```
serve
```

会在运行前变为：

```
python manage.py runserver
```

别名对于冗长或重复的命令以及你总是使用的带标志的命令最有帮助。例如：

```
alias ll="ls -la"
alias migrate="python manage.py migrate"
alias test="python manage.py test"
```

现在你的工作流变为：

```
serve
migrate
test
```

### 临时与永久别名

如果你在终端中定义别名，它仅在该会话期间有效。如果你关闭终端，它就会消失。

要使别名永久生效，请将其添加到你的 Shell 配置文件中：

*   **Bash**：`~/.bashrc` 或 `~/.bash_profile`
*   **Zsh**：`~/.zshrc`

例如，打开你的配置文件并添加：

```
alias serve="python manage.py runserver"
```

然后重新加载它：

```
source ~/.bashrc
```

现在每次打开新终端时该别名都将可用。

别名是简单的文本替换。它们非常适合快捷方式，但受限于无法轻松处理参数，因此不适合多步骤例程。它们还可能使命令对其他开发者不够直观，因为它们不是标准的。

如果快捷方式变得复杂或需要逻辑，通常最好将其转换为脚本。

别名是让命令行感觉不那么重复的最快方法之一。将别名视为快捷方式，将脚本视为可重复的过程。

## 命令无情

Bash 功能强大但不宽容。图形文件管理器通常会在删除前询问。Bash 往往会立即完全执行你所要求的操作。这一点不是为了吓唬你，只是为了鼓励你在按下 `Enter` 键前仔细阅读命令的习惯，尤其是从文档或 AI 输出复制命令时。

引入 Bash 不是为了让你成为 Shell 专家。而是为了让你能遵循说明、理解工具在做什么，并在重复变得烦人时编写小脚本。

Bash 是一个在接受 AI 生成的答案前放慢脚步的好地方。像 `rm`、`chmod` 或 `docker run` 这样的命令可以更改文件、权限、进程或网络访问。在运行之前，请检查它将触及什么以及是否取决于你的当前目录。

运行命令前，请问：我在文件系统中的什么位置？这将触及哪些文件？它是创建、删除、更改权限、启动进程还是通过网络发送数据？这种思考比从记忆中输入命令更重要。命令行奖励理解行为的开发者。

许多读者将使用 Windows，这是个问题，因为 Bash（或类似 Bash 的替代 Shell，如 Mac 上的 Zsh）不是 Windows 的标准组件。如果你使用 Windows 且没有 Bash 或类似工具，在下一节你将学习如何安装它。如果你不使用 Windows，可以跳过下一节，继续阅读“基本命令”部分。

# 在 Windows 上设置 Bash

要在 Windows 上使用 Bash，请安装 **Windows Subsystem Linux**（**WSL**）。

WSL 2 支持 Windows 11 和 Windows 10 版本 1903（内部版本 18362）或更高版本。

## 从提升的终端安装 WSL

以管理员身份打开 PowerShell，然后运行：

```
wsl --install
```

或者，要安装特定的 Linux 发行版（如 Ubuntu），请运行：`wsl --install -d Ubuntu`。要列出可用的发行版，请运行：`wsl --list --online`。

这将安装 WSL，并根据你的 Windows 版本和配置通常安装默认的 Linux 发行版（通常是 Ubuntu），如以下输出所示：

```
Downloading: Ubuntu
[==========================53.5%                           ]
```

如果你已经安装了 WSL，请检查你拥有的内容：

```
wsl -l -v
```

这将列出已安装的发行版以及每个是 WSL 1 还是 WSL 2，如以下输出所示：

```
  NAME              STATE           VERSION
* docker-desktop    Stopped         2
  Ubuntu            Running         2
```

## 启动你的 Linux 发行版并完成首次设置

打开 **开始** 菜单，运行你的发行版，例如 **Ubuntu**，并按照提示创建 Linux 用户名和密码。这独立于你的 Windows 登录。

从现在起，当你启动 **Ubuntu** 时，你应该会看到如 *图 10.1* 所示的 Bash 提示符：

![](Images/B34553_10_01.png)

图 10.1：Ubuntu Bash 提示符和终端窗口

## 在 Linux 内部更新包

在 WSL 终端中，运行：

```
sudo apt update
sudo apt upgrade -y
```

这使环境保持最新，并避免令人困惑的安装问题。

**良好实践**：`sudo` 以管理员（root）权限运行命令。当你使用它时，系统会要求输入你的 Linux 密码以确认操作。此级别的访问应谨慎使用，仅用于系统级任务，例如使用 `apt` 安装软件。对于日常开发命令，如 `pip install`、`python` 或 `npm`，你不应需要 `sudo`。如果这些工具仅在以提升的权限运行时才工作，通常意味着你的环境或权限设置不正确，应修复而不是绕过。

## 确认 Bash 正在运行

在 WSL 终端中，运行：

```
echo "$SHELL"
```

你应该会看到类似 `/bin/bash` 的路径（或类似内容）。

在 WSL 终端中，运行：

```
bash --version
```

你应该会看到版本输出（包含版权和许可证信息）：

```
GNU bash, version 5.2.21(1)-release (x86_64-pc-linux-gnu)
```

## 为本章创建练习文件夹

在 WSL 终端中，运行：

```
mkdir -p ~/bash-chapter
cd ~/bash-chapter
```

在 WSL 终端中，检查你所在的位置：

```
pwd
```

你应该会看到你位于 `home`，然后是你的用户名（如我的 `markjprice`），然后是你刚创建的新目录：

```
/home/markjprice/bash-chapter
```

在 WSL 终端中，运行：

```
ls -la
```

如果你完成了 Bash 练习，在 WSL 终端中，你可以运行：

```
exit
```

在 Windows 文件资源管理器中，你可以使用 Linux 节点查看你的 WSL 安装，如 *图 10.2* 所示：

![](Images/B34553_10_02.png)

图 10.2：Windows 文件资源管理器及其 WSL 集成

这些命令的行为与本章描述的完全一致。

现在让我们通过学习一些常见的基本命令将这些背景知识付诸实践。

# 基本命令

在编写脚本或自动化任何事情之前，你需要一小套日常命令词汇。这些是你在处理项目、遵循教程或从终端调试问题时将持续使用的命令。你不需要记住每个选项。知道每个命令的用途比知道它支持的每个标志要重要得多。

我们将在本章中使用的核心命令有：

*   `pwd` – 打印当前工作目录
*   `ls` – 列出文件和目录
*   `cd` – 更改当前目录
*   `mkdir` – 创建新目录
*   `touch` – 创建空文件
*   `cp` – 复制文件和目录
*   `mv` – 移动或重命名文件和目录
*   `rm` – 删除文件和目录
*   `cat` – 打印文件内容
*   `echo` – 向终端输出文本
