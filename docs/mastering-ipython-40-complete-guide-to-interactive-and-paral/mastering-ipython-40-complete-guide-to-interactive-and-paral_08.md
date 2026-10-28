# Notebook

Jupyter Notebook 是造成这次重大分裂的主要原因。回顾 IPython 4.0 之前各版本的发行说明，可以清楚地看出，许多改进针对的是 Notebook 组件，而非 Python 专属功能，甚至也不是底层架构。

Notebook 本身规模庞大，拥有自己的子项目：

- 文档格式（及其 API，nbformat）
- nbconvert
- nbviewer

Notebook 的主要功能是显示信息——计算则交由内核完成——因此这一层似乎不太可能出现兼容性问题。不过，Notebook 提供的功能不止信息显示，而且功能还在不断增加，因此兼容性问题仍有可能出现。

# 控制台

控制台是一种命令行界面，可以与遵循 Jupyter 交互式计算协议的任何内核通信。目前尚不清楚，既然 IPython 的终端如今已与语言无关，为何还需要一种新的终端界面，而不直接使用 IPython 的终端。从用户的角度来看，它们看起来确实一样。实际上，它们可能是同一个东西，只是将一些 IPython 专属代码替换成了与 Jupyter 兼容的代码。文档说明了如何启动和使用控制台，却没有解释它与 IPython 的关系（如果存在这种关系的话）。快速搜索网络也无济于事，甚至有些网站声称控制台就是新版 IPython。

# Jupyter 客户端

Jupyter Client 包是一个 Python API，用于启动、管理内核并与之通信。IPython 的内核也遵循这一标准。它规定了所有内核必须遵循的消息传递协议，以便与 Notebook 通信。作为 Jupyter 的参考内核，IPython 内核必定会继续遵循这一规范。
