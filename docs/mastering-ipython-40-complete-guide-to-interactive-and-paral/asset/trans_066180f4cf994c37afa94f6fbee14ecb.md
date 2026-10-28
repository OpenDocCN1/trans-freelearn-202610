

## 为源文件编写文档

遗憾的是，与 Javadoc 不同，Docutils 无法接受包含大量 Python 代码的 .py 文件，并自动从中提取文档字符串以生成文档。有一个名为 Epydoc 的工具可以做到这一点，但该项目自 2009 年以来一直处于不活跃状态。团队可以编写脚本来尝试仅提取文档字符串，但还有更好的解决方案。这将在下一节中概述。

## Sphinx

Sphinx 是一个文档生成工具。它最初用于生成 Python 文档，但目前已扩展到其他语言，尤其是 C/C++。它可以生成多种格式的输出，包括 HTML、LaTeX、ePub、man 页面和纯文本等。它接受 reStructuredText，并在 Docutils 套件的基础上构建，同时添加了一些标记。

## 安装与启动

可以使用 `pip` 安装 Sphinx：

```
pip install sphinx
```

安装完成后，会将多个脚本添加到路径中。我们要介绍的第一个脚本是 `sphinx-quickstart`。它会提示用户输入信息，然后创建源目录、配置文件和某些骨架文件：

```
bash-4.1$ sphinx-quickstart
Welcome to the Sphinx 1.3.6 quickstart utility.

Please enter values for the following settings (just press Enter to accept a default value, if one is given in brackets).

Enter the root path for documentation.
> Root path for the documentation [.] : docs

You have two options for placing the build directory for Sphinx output.
Either, you use a directory "_build" within the root path, or you separate
"source" and "build" directories within the root path.
```

## 文档

这将生成如下页面：

![](img/8628b86fcc4f22d859c0104c2ec1d15f_324_0.png)

页面中缺少对 `chinese_remainder` 方法的文档，因此可以使用 automethod 指令将其添加到 `index.rst` 中：

```rst
.. toctree::
   :maxdepth: 2

.. automodule:: crt1
```

虽然这样很灵活，但可能繁琐且容易出错。一种解决方案是使用 `:members:` 属性，如下所示：

```
    .. toctree::
       :maxdepth: 2

    .. automodule:: crt1
       :members:
```

另一种方法是使用 `sphinx-apidoc` 工具。`sphinx-apidoc` 的作用是从源文件中提取文档字符串，并根据它们生成 `.rst` 文件。请参考以下示例：

```
bash-4.1$ sphinx-apidoc -o docs .
```

它会生成一组 `.rst` 文件：

```
Creating file docs/code1.rst.
Creating file docs/code2.rst.
Creating file docs/crt1.rst.
Creating file docs/hail1.rst.
Creating file docs/modules.rst.
```

这些文件本身没什么可看的，但它们可以输入到 Sphinx 或其他期望 reStructuredText 的文档工具中，以进行进一步处理。当使用本身无法提取文档字符串的工具时，这一点尤其方便。

## 总结

虽然内联注释和类型标注对于将在代码库上工作的开发人员很有用，但外部人员需要更全面的文档，并且需要一种比阅读源代码更具吸引力的格式（尤其是当他们无法访问源代码时）。对文档的需求必须与开发人员可以理解的、不愿花费大量时间和精力去编写代码以外任何东西的心理相平衡。

Python 的解决方案是采用轻量级标记语言 reStructuredText 和工具链（Docutils、Sphinx），将 reStructuredText 转换为外观更精美的格式（例如 HTML 或 LaTeX）。

第 3 章“迈入 IPython 进行并行计算”提供了一些细节，说明如何拆分内核和 REPL 协议，从而使并行架构得以出现。

本章将提供一些示例，展示 notebook 的强大功能，以及它与其他组件的关系。从 IPython notebook 到 Jupyter 项目的转变可能是最大的一次重组，被称为“大分裂”（The Big Split）。如需深入探讨“大分裂”的原因及其对各种包结构的影响，请参阅 https://blog.jupyter.org/2015/04/15/the-big-split/。

## 安装与启动

Jupyter 可以使用 pip 安装：

```
pip install jupyter
```

可以像这样启动：

```
(my_rpy2_zone)-bash-4.1$ ipython notebook
[I 10:38:27.193 NotebookApp] Serving notebooks from local directory: /nfs/02/wit0096
[I 10:38:27.193 NotebookApp] 0 active kernels
[I 10:38:27.193 NotebookApp] The Jupyter Notebook is running at: http://localhost:8888/
[I 10:38:27.193 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
/usr/bin/xdg-open: line 402: htmlview: command not found
console.error:
  [CustomizableUI]
  Custom widget with id loop-button does not return a valid node
console.error:
  [CustomizableUI]
  Custom widget with id loop-button does not return a valid node
1457711201438  addons.update-checker  WARN  Update manifest for {972ce4c6-7e08-4474-a285-3208198ce6fd} did not contain an updates property
```

注意，这会在 8888 端口启动一个 HTML 服务器（称为 Jupyter Notebook 应用）。它可能还会（取决于你的平台）启动指向 http://localhost:8888 的浏览器。如果没有，请手动启动浏览器并指向该地址。Jupyter 只能访问其启动目录（及其子目录）中的文件，因此请务必在其能够访问你的 notebook 文件的位置启动它。

## 创建 notebook

当然，没有人会用 JSON 编写 notebook——它只是用作持久化和交换的文本格式。可以使用仪表盘（Dashboard）创建 notebook。在界面的右上部分有一个下拉框，提供创建新 notebook 的选项：

![](img/8628b86fcc4f22d859c0104c2ec1d15f_336_0.png)

## 使用 matplotlib

在 notebook 中使用 matplotlib 的关键是 `%matplotlib inline` 魔术命令。它应该是单元格中的第一行。例如：

![](img/8628b86fcc4f22d859c0104c2ec1d15f_344_0.png)

第一个单元格（打印 **图形很有趣**）是 Markdown；第二个单元格创建了所示图表。

## 使用 R

使用 R 时，重要的是所有工作都应在 R 中完成（或者至少是实际显示图表的部分）。否则，图表将在单独的窗口中打开，而不是在 notebook 中。%%R 单元格魔术是完成这项工作的正确工具。特别是，应该打印图表，而不是使用 plot( ) 函数。例如：

![](img/8628b86fcc4f22d859c0104c2ec1d15f_346_0.png)

IPython 不是这样的系统。2001 年，Fernando Perez 是科罗拉多大学博尔德分校的一名研究生。他不是计算机科学专业，而是一名需要编程并希望有更好工具的物理学研究生。作为一名物理学家，他对“更好”的想法与计算机科学家可能设想的不同。特别是，对早期 IPython 形成影响的是主要为数据分析和展示而设计的工具：Mathematica (http://www.wolfram.com/mathematica/) 和 Sage (http://www.sagemath.org/)。

Perez 曾表示，IPython“旨在成为一个高度可配置的工具，用于构建针对特定问题的交互式环境”，这为其历史和未来方向提供了一把钥匙。

最初的努力从两个方向入手。一方面，当时已经存在几个 Python 扩展——IPP（由 Janko Hauser 开发）和 LazyPython（由 Nathan Gray 开发）。另一方面，Perez 对自己应该实现什么有自己的想法。最终，IPython 诞生于 IPP、LazyPython 和 Perez 自己努力的代码合并。

此时开发速度放缓了。有人尝试过制作 notebook。也曾考虑过与 Sage 合并，但最终项目各自发展。2009 年，第一位全职开发者获得了资助。他的第一项任务是重构整个代码库，当时它仍然是 IPP、LazyPython 和 Perez 原始代码的大杂烩。虽然没有添加新功能，但在此后干净整洁的代码上，开发可以更容易地推进。

2010 年是重要的一年。第一个重大发展是转向基于 ZeroMQ 的架构。采用 ZeroMQ 带来了两项重要的架构进步：前端与解释器的分离，以及对并行性的支持。

分离前端和后端在多个方面意义重大。也许最重要的是，这意味着实现 notebook 变得更简单：Notebook 只是后端 Python 内核的另一个接口。另一方面，命令行界面可以自由连接到支持任何语言（不仅仅是 Python）的内核。IPython 正从“一个更好的 Python shell”转变为“一个多接口、多语言的平台”。此时工作进展迅速：2010 年剩余时间里，开发了命令行界面、Qt 控制台、与 Visual Studio 的 Python 工具集成，以及一个 Web notebook 原型。2011 年，该原型变成了完全可用的版本，IPython 0.12 发布。

