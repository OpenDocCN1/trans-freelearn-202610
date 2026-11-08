
## 存储与重用

作业和转换可以保存在存储中以便将来重复使用。Pentaho Data Integration 用于保存作业和转换的格式是 XML，因此可以保存到文件中。作业保存在扩展名为 `.kjb` 的文件中，而转换则保存在扩展名为 `.ktr` 的文件中。

由于它们是文件，作业和转换自然可以保存到文件系统中，但这并不是 `.kjb` 和 `.ktr` 文件可以存储的唯一位置。Pentaho Data Integration 能够从以下类型的存储中检索和存储作业与转换：

- **Pentaho 仓库**：你在第 15 章 *在 Pentaho Business Analytics Platform 中使用报表* 中已经了解过 Pentaho 仓库。从用户角度来看，Pentaho 仓库可作为由 Pentaho Business Analytics Platform 拥有的远程文件系统使用。Pentaho 仓库能够存储、检索和执行 `.kjb` 和 `.ktr` 文件。
- **数据库**：这是一个常规数据库，其中包含一些在创建时由 Pentaho Data Integration 构建并在检索、保存和执行期间管理的表。作业和转换不会以其 XML 格式保存，而是作为多行存储在多个自定义表中。
- **文件系统**：你已经了解到，使用文件系统时，作业和转换以 XML 格式保存为 `.kjb` 和 `.ktr` 文件。

![](img/9337ac94537bf7e092e1ea6531dce8fd_673_0.png)

> *即使支持三种不同的存储类型，也建议使用 Pentaho 仓库，因为使用 XML 格式存储作业和转换是标准做法（数据库存储不可读），并且它是集中化的且可通过 API 访问（文件系统仅限于服务器/机器本地）。*

## Spoon 客户端介绍

PDI 客户端（Spoon）是你安装在工作站上的桌面应用程序，可让你构建转换和作业或安排作业的运行时间。Spoon 包含在标准的 Pentaho Data Integration 发行版中，可以使用 `data-integration` 文件夹中的 `spoon` 脚本启动（`spoon.sh` 用于基于 Linux 的操作系统，`spoon.bat` 用于基于 Windows 的操作系统），如下所示：

![](img/9337ac94537bf7e092e1ea6531dce8fd_674_0.png)

你不会在本节中发现有关 Spoon 的所有细节，因为深入研究它确实需要一整本书。相反，我们想分享的是，通过图形界面，你可以直观地看到转换和作业中的流程，以及对它们的执行进行调试的完全控制。本章中展示的所有图像都是使用 Spoon 完成的，在以下章节中你将看到其实际用法的示例。

## Pentaho Reporting 输出步骤

Pentaho Reporting 输出步骤是唯一完全专用于 Pentaho Reporting 的 Data Integration 任务。作为一个步骤，它存在于转换中，并被归类在输出步骤中。

该步骤的主要目标是基于文件系统上本地存储的 `.prpt` 文件生成 Pentaho 报表。作为一个常规步骤，它接受数据流作为输入，并对流中的每一行执行任务。Pentaho Reporting 输出步骤可以直接从 Spoon 界面进行自定义，方法是右键单击它并选择编辑项。

在下面的屏幕截图中，你可以看到编辑窗口在 Spoon 界面中的外观：

![](img/9337ac94537bf7e092e1ea6531dce8fd_675_0.png)

在下表中，你可以阅读每个选项的简要描述，作为管理 Pentaho 报表的集成数据能力介绍：

| 选项 | 描述 |
|---|---|
| Step name | 步骤的名称。它在转换中必须是唯一的。 |
| Report definition file | 包含执行期间 .prpt 文件路径（包括文件名）的输入流字段。 |
| Output file | 包含执行期间输出文件路径（包括文件名）的输入流字段。 |
| Parameter fields | 你可以将一个或多个输入字段的值传递给报表中的预定义参数。要将数据传递给字符串数组，请确保数据以制表符分隔。你可以使用“获取报表参数”按钮从现有报表导入参数。 |
| Output processor | 用于生成输出文件的处理器，每一个对应一种可能的输出格式。 |

在本章描述的示例中，你将看到 Pentaho Reporting 输出步骤的具体使用。

## 介绍 Pan 和 Kitchen

为了完成对 Pentaho Data Integration 主要功能简要介绍，让我们看看如何从命令行执行通用作业或转换。为此，提供了两个名为 Pan 和 Kitchen 的 Pentaho 工具。

Kitchen 是一个可以执行由 Spoon 设计的作业的程序。Pan 是一个可以执行由 Spoon 设计的转换的程序。通常作业和转换以批处理模式安排，定期自动运行。Kitchen 和 Pan 捆绑在 Pentaho Data Integration 的标准发行版中，作为 `data-integration` 文件夹中的名为 `kitchen` 和 `pan` 的脚本提供（`kitchen.sh` 和 `pan.sh` 用于基于 Linux 的操作系统，`kitchen.bat` 和 `pan.bat` 用于基于 Windows 的操作系统）。

`kitchen` 和 `pan` 脚本可以使用多个参数启动，以控制和配置其执行。参数可以在命令行中指定，使用语法 `/param:value` 或 `-param=value`。

在下表中，你可以找到 `kitchen` 和 `pan` 脚本接受的主要参数：

| 参数 | 描述 |
|---|---|
| `rep` | 如果你使用的是 Pentaho 仓库名称或数据库。 |
| `user` | 如果你使用仓库或数据库，则为用户名。 |
| `pass` | 如果你使用仓库或数据库，则为密码。 |
| `trans` | 要启动的转换名称（在仓库中显示的名称）。仅适用于 pan。 |
| `job` | 要启动的作业名称（在仓库中显示的名称）。仅适用于 kitchen。 |
| `dir` | 包含转换/作业的路径，包括前导斜杠。 |
| `file` | 如果你调用本地 .ktr/.kjb 文件，则为路径和文件名。 |
| `level` | 日志级别（Basic、Detailed、Debug、Rowlevel、Error 和 Nothing）。 |
| `logfile` | 写入日志输出的本地文件名。 |
| `listdir` | 列出指定仓库中的目录。 |
| `listtrans` | 列出指定仓库目录中的转换。仅适用于 pan。 |
| `listjob` | 列出指定仓库目录中的转换。仅适用于 kitchen。 |
| `listrep` | 列出可用的仓库。 |
| `exprep` | 将所有仓库对象导出到一个 XML 文件。仅适用于 pan。 |
| `export` | 导出指定作业的所有关联资源。参数是 ZIP 文件的名称。仅适用于 kitchen。 |
| `norep` | 防止登录到仓库。如果不想执行本地 `.ktr/.kjb` 文件，则应设置环境变量 `KETTLE_REPOSITORY`、`KETTLE_USER` 和 `KETTLE_PASSWORD`。 |
| `safemode` | 以安全模式运行，可启用额外检查。仅适用于 `pan`。 |
| `version` | 显示版本、修订号和构建日期。 |
| `param` | 以 `name=value` 格式设置命名参数。例如，`-param:F00=bar` |
| `listparam` | 列出指定转换中定义的命名参数的信息。 |
| `maxloglines` | Pentaho Data Integration 内部保留的最大日志行数。设置为 `0` 以保留所有行（默认值）。 |
| `maxlogtimeout` | Pentaho Data Integration 内部保留时日志行的最大年龄（以分钟为单位）。设置为 `0` 以无限期保留所有行（默认值）。 |

