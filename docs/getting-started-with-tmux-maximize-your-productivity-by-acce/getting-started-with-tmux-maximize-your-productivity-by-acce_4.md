## 附录

附录将涵盖以下三个主题：

- 为什么使用 tmux？
- 配置参考
- 键绑定和命令参考

### 为什么使用 tmux？

许多开发人员在终端中花费了大部分时间。无论是用于编码；SSH 连接到远程服务器；浏览文件系统；本地任务，如检查、编译或代码检查、运行单元测试；甚至用于邮件或 Internet Relay Chat（IRC），终端都是开发人员武器库中最广泛使用的工具之一。

tmux 是一个在终端内运行的命令行应用程序，可以 turbocharge 它。其强大的功能允许简化许多日常任务，如本书各处所示。然而，大多数人使用像 tmux 这样的程序的主要原因是它允许你将一个终端窗口变成许多虚拟窗口，每个窗口都有自己的状态。它属于一类称为终端多路复用器（tmux 只是该术语的缩写）的应用程序。它有一些兄弟程序，但最突出的是 GNU Screen。

还记得没有标签页的网页浏览器吗？如果你回答没有，你就没错过什么，并且享受你的青春！然而，如果你回答是，可能需要一秒钟来回忆它有多不方便。当然，用多个标签页可以做的事情，用没有标签页和多个浏览器窗口也可以做，但能够将它们组合在一起仍然是对生产力的很大提升。拥有许多浏览器窗口会非常混乱。标签页的出现让你能够在逻辑上分离你的浏览体验。在标签页之前，你会有用于参考文档的一个浏览器窗口，另一个用于购物，等等。

同样，tmux 等终端多路复用器允许你做同样的事情，将多个窗口在逻辑上分组到一个终端窗口中，就像浏览器中的标签页一样。就像每个浏览器标签页包含自己的状态一样，每个 tmux 窗口也是如此。

然而，与浏览器标签页不同，后者主要只是增加了组织性，tmux 实际上为终端增加了丰富的额外功能。tmux 不仅允许将窗口分割成窗格，以便屏幕上同时显示多个不同的内容，而且 tmux 还增加了复制模式和其他结构，允许重度终端用户在不离开 tmux 或甚至不必经常使用鼠标的情况下做更多事情。我们在本书中触及了其中的许多好处，因此在这里就不必重复了。

为什么要在标准终端应用程序带原生标签页（例如，OS X Terminal、iTerm、Gnome Terminal 等）的情况下使用终端多路复用器？原因有很多，但以下只是其中几个：

- 复制模式的添加允许在终端窗口之间复制和粘贴，而无需使用鼠标。这对于标准标签页终端应用程序来说很难实现。
- 保存状态的添加意味着你可以关闭正在运行 tmux 的终端窗口，你的整个会话状态将被保存并可以在以后重新附加（假设你的终端程序允许 tmux 服务器在后台继续运行；并非所有都允许）。任何正在运行的程序都会在后台继续运行。这对于 iTerm、OS X Terminal 或任何 Linux 终端中的标签页来说并非如此。关闭其中一个将丢弃该窗口的状态。
- 使用 tmux 将窗口分割成窗格并重新排列这些窗格的强大功能和灵活性远超任何标签页终端界面。大多数现代终端应用程序现在只支持简单的垂直或水平分割，大概就这样了。而对于 tmux 而言，分割的数量、布局等方面的可能性几乎是无限的。
- 所有用于更改这些分割的大小和布局的 tmux 键盘快捷键都意味着对鼠标的依赖减少，这是大多数普通标签页终端应用程序无法提供的。
- 不仅 tmux 提供了更多用于调整这个或那个大小的快捷键，而且 tmux 还提供了更广泛的自定义范围，针对它能处理的每个操作的键盘快捷键。实际上，每个 tmux 快捷键都可以通过简单的配置文件更改或重新连接，如第 2 章《配置 tmux》所示。一些终端应用程序可能提供一些自定义，但没有一个在这一点上像 tmux 那样强大。
- tmux 中的窗格和窗口管理要简单得多，也更强大。使用 tmux，很容易将窗口分割成许多不同的窗格，移动它们，将它们附加到不同的窗口，移动窗口，将窗口附加到不同的会话，等等。Screen 中许多这些任务是不可能的，而且最终比使用 tmux 复杂得多。
- tmux 的状态栏配置开箱即用非常简单，默认设置大多数人都会喜欢。另一方面，Screen 默认不包含状态栏，并且指定它的方法非常复杂。例如，这是我 Screen 配置中启用 tmux 风格状态栏的实际一行：

  ```
  hardstatus string '{%=kG} [ %{G}%H %{g} [%{=kw}%?%-Lw%?%{r}(%{W}%n*%f%t%? (%u)%?%{r})%{w}%?%+Lw%?%= %{g}] [%{B}%Y-%m-%d %{W}%c %{g}]'
  ```

不；这不是一个笑话。这是实际的行，它所做的只是显示 tmux 状态栏开箱即用支持的相同内容。

tmux 相对于 Screen 的好处还有更多，但这应该能让任何读者很好地了解这里的理由。

> 作为我们曾经使用过的第一个终端多路复用器，Screen 在作者心中仍占有特殊地位。这里的意图绝对不是抨击 Screen，该程序令人印象深刻地在撰写本文时庆祝其第 27 年。Screen 仍然是一个非常棒的程序，并且在很大程度上为 tmux 铺平了道路。然而，你购买这本书是因为你想学习 tmux，我们的意图是列举一些原因，说明为什么 tmux 是其类型中最棒的程序。

![](img/5969098f450265ac0c7947efcdbe7889_134_0.png)

### 配置参考

下面是我们在第 2 章《配置 tmux》中逐步构建的配置文件：

```
# Enable mouse mode for mouse scrolling (tmux 1.9a+)
set-window-option -g mouse-mode on
# Enable mouse for selecting the window by clicking on the title
# in the status bar
set-option -g mouse-select-window on
# Enable mouse for selecting the pane by clicking on it
set-option -g mouse-select-pane on
# Enable the mouse for clicking and dragging to resize panes
set-option -g mouse-resize-pane on
```




# Set the status bar background to blue
set-option -g status-bg blue
# Set the status bar text to white
set-option -g status-fg white

# Widen the status-left a bit to fit more
set-option -g status-left-length 25
# Change status-left to be {username@host}
set-option -g status-left "#{whoami}@#H"

# Set the active window background in the status bar
set-window-option -g window-status-current-bg magenta

# Rebind the prefix key
set-option -g prefix C-t

# Add a key binding for reloading our configuration
bind-key C-r source-file ~/.tmux.conf
# Double tapping the prefix jumps to last window
bind-key C-t last-window

# Set escape time to not break vim
set-option -s escape-time 0

# Set the base-index to 1 rather than 0
set-option -g base-index 1
set-window-option -g pane-base-index 1

# Extend the display time to 2 seconds
set-option -g display-time 2000

# Store more history in the buffer than default
set-option -g history-limit 10000

## 键绑定与命令参考

这并非旨在成为 tmux 键绑定和命令的详尽参考。若需详尽参考，可在终端中输入以下命令查看 tmux 手册页：

```
$ man tmux
```

你也可以访问 http://man.cx/tmux 查看更多有关 tmux 的信息。

相反，这是一份逐章参考，涵盖了每章中提到的 tmux 键和命令，并按照它们被提及的顺序附有简要描述。

这样便于你按照章节中出现的顺序回顾和记忆这些键绑定和命令，而无需通读整章内容。

### 第 1 章 – 直接上手

在本章中，我们直接上手，游览了 tmux 的许多功能。因此，以下命令列表五花八门：

- 启动 tmux：$ tmux
- 重命名会话：$ tmux rename-session {session name}
- 创建新窗口：<Prefix>, c
- 切换到最后一个窗口：<Prefix>, l
- 按索引切换窗口：<Prefix>, {index of window}
- 列出所有键绑定：<Prefix>, ?
- 在键绑定列表中启动搜索：Ctrl + s（或 vi 用户使用 /）
- 关闭 tmux 打开的任何对话框，例如键绑定列表：q
- 打开选择窗口界面：<Prefix>, w
- 切换到下一个窗口（按窗口索引）：<Prefix>, n
- 切换到上一个窗口（按窗口索引）：<Prefix>, p
- 使用指定搜索文本查找打开的窗口：<Prefix>, f
- 脱离当前 tmux 会话：<Prefix>, d
- 列出所有活动会话：$ tmux list-sessions
- 启动 tmux 并按名称附加会话：$ tmux attach-session -t {session-name}
- 启动 tmux 并附加会话的更短方式：$ tmux attach -t {session-name}
- 启动 tmux 并附加会话的更更短方式（仅当只有一个活动会话时可用）：$ tmux attach
- 列出所有活动会话的别名：$ tmux ls
- 列出所有 tmux 命令：$ tmux list-commands
- 列出所有 tmux 命令的别名：$ tmux lscm

### 第 2 章 – 配置 tmux

在本章中，我们大量涉及配置。这里列出的是*第 2 章“配置 tmux”*中介绍的键绑定和命令，关于我们讨论的新配置项，请参阅*配置参考*部分：

- 设置 tmux 选项：`$ tmux set-option {option to set} {value}`
- 禁用状态栏：`$ tmux set-option status off`
- 启用状态栏：`$ tmux set-option status on`
- 查看当前处于 Emacs 还是 vi 模式：`$ tmux show-options -g | grep key`
- 重新加载 tmux 配置：`$ tmux source-file ~/.tmux.conf`
- 显示所有先前显示过的消息：`<Prefix>, ~`

### 第 3 章 – 会话、窗口和窗格

在*第 3 章“会话、窗口和窗格”*中，我们学习了如何使用 tmux 的三个核心构建块——会话、窗口和窗格——在终端窗口内移动和组织内容。因此，我们在本章中学习的键命令都与操作这三个元素之一有关。本章涵盖的键命令如下：

- 使用指定名称的会话启动 tmux：`$ tmux new-session -s {session name}`
- 访问切换会话的交互式对话框：`<Prefix>, s`
- 切换到下一个会话：`<Prefix>, )`
- 切换到上一个会话：`<Prefix>, (`
- 将一个窗格水平拆分为两个窗格：`<Prefix>, %`
- 将光标切换到另一个窗格：`<Prefix>, o`
- 将光标移动到右侧、左侧、下方或上方的窗格：`<Prefix>, right arrow; <Prefix>, left arrow; <Prefix>, down arrow; <Prefix>, up arrow`
- 关闭当前窗格：`<Prefix>, x`
- 将一个窗格垂直拆分为两个窗格：`<Prefix>, "`
- 调整当前窗格大小：`<Prefix>, Alt + {arrow key}`

- 按索引查看粘贴缓冲区的内容：`show-buffer -b {index}`
- 从粘贴缓冲区删除最后复制的项目：`<Prefix>,-`
- 按索引从粘贴缓冲区删除项目：`delete-buffer -b {index}`
- 清除当前窗格的 tmux 历史记录：`clear-history`
- 按索引清除某个窗格的 tmux 历史记录：`clear-history -t {index}`
- 将窗口从一个会话移动到另一个会话：`move-window` 或 `<Prefix>,.`
- 在两个会话之间链接窗口：`link-window -t {target session}`
- 从当前会话取消链接窗口：`unlink-window`
- 将当前窗格从当前窗口中拆出：`<Prefix>,!`
- 将窗格拆分为独立窗口：`break-pane -s {session}:{window}.{pane}`
- 将当前窗格加入目标窗口：`join-pane -t {session}:{window}`
- 将一个窗格加入目标窗口：`join-pane -s {session}:{window}.{pane} -t {session}:{window}`
