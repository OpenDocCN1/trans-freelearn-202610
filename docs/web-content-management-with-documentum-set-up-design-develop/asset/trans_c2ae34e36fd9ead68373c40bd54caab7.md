
## Monitor_Lifecycles 作业在 Documentum 系统中的作用是什么？
Documentum 中的 Monitor_Lifecycles 作业按周期性间隔运行，如果其生效日期与当前系统日期匹配，则将 Approved 对象转为 Active，并如果其过期日期与当前系统日期匹配，则使 Active 对象过期。

## 哪个系统属性有助于区分模板文件和内容文件？
a_is_template 属性：该属性的值对于模板文件为 1，对于使用该模板创建的内容文件为 0。

## Web Publisher 编辑器中有哪些各种小部件类型？
Web Publisher 编辑器中可用的各种小部件有：textline、content、choice、textselector、graphic、checkbox、xselector 和 repeating blocks。

## Web Publisher 编辑器中的 Repeating Block 小部件是什么？
Web Publisher 编辑器中的 Repeating Block 小部件允许内容作者在 Web Publisher 编辑器运行时创建并随后处理封闭小部件（例如 textline、content 等）的多个实例。

## 可以将多个 Rules 文件与单个 Web Publisher 模板文件关联吗？
可以，系统允许您将多个 Rules 文件与单个 Web Publisher 模板文件关联。

## Textselector 小部件在 Web Publisher 编辑器中有什么用途？
Web Publisher 编辑器中的 textselector 小部件允许从已上传到 Docbase 的文件中选择一段文本。

## Web Publisher 中演示文件的两种类型是什么？
Web Publisher 中的演示文件有两种类型：Editor Presentation 文件（XSL 样式表）和 External Presentation 文件（HTML 包装器）。

Editor Presentation 文件用于格式化/设置 Web Publisher 编辑器 XML 内容文件的样式，并存储在 Web Publisher 的 Site Manager | Presentations | Editor 文件夹下。

另一方面，External Presentation 文件为嵌入在 HTML 内容文件 <body></body> 标签中的内容提供标准框架和包装器。它们存储在 Site Manager | Presentations | External 文件夹下。

## Web Publisher 中的 Reapply presentation 功能提供什么好处？
Web Publisher 中的 Reapply presentation 功能允许您根据修改后的演示文件更新 Docbase 中现有内容的呈现版本。

## 什么是 XDQL？
XDQL 是 eXtensible Document Query language 的缩写，是 Documentum 中的一种机制，允许您在 XML 内执行 DQL 查询并通过 XML 处理查询结果。

## Web Publisher Create_Dynamic_Content 作业的用途是什么？
Web Publisher Create_Dynamic_Content 作业可以设置为按周期性间隔运行，以转换处于 Active 状态且其模板的 Content Refresh 标志设置为 true 的内容文件。该作业对 Active 内容文件重新应用演示文件，从而确保正确的 XDQL 查询结果保存到转换后的输出 Web 呈现中。

## 什么是 Web Publisher Folder Map？
Web Publisher Folder Map 是一个扁平的 XML 文件，其中包含定义 Docbase 属性（属性）属性匹配条件的规则，基于这些规则，内容可以在 Web Cabinets 中的指定文件夹位置下创建。

例如，Folder Map 可以定义一条规则，规定所有 title 属性不为空的内容都应创建在 Web Cabinet 频道文件夹 TitledContents_Fld（假设）下。

## Folder Map 能否配置为基于指定的匹配属性条件将相同内容创建在两个不同的文件夹位置下？
可以。您可以在 <path> 标签中为给定的属性匹配条件指定两个文件夹位置。

## 如果在 Folder Map 中指定的文件夹位置或 Web Cabinet 名称在 Docbase 中不存在会怎样？
如果指定的 Web Cabinet 在 Docbase 中不存在，Folder Map 会抛出错误。但是，如果指定的文件夹（在现有 Web Cabinet 内）不存在，则 Folder Map 会自动创建它们。

## 什么是动态文件夹映射？
Folder Map 可用于使用 $(...) 记法根据指定的属性值在运行时自动创建名称基于这些值的文件夹。例如，您可以配置 Folder Map，以便在运行时在新创建的文件夹内创建内容，该文件夹的名称取自内容 title 属性提供的值。

例如：为 title 属性指定的值：Dummy Title

Folder Map 中指定的文件夹路径：/<web Cabinet name>/${title}

在运行时，系统会在指定的 Web Cabinet 下创建一个名为 Dummy Title 的文件夹。

## Instruction Files 在 Web Publisher 中的用途是什么？
Web Publisher 中创建的 Instruction Files 是扁平的 XML 文件，可用于根据模板文件的更新结构自动创建或更新现有内容 XML 文件的结构，而无需手动更新现有内容。

## 请列出 Web Publisher 中 Instruction Files 可以执行的几个 XML 操作。
Web Publisher 中的 Instruction Files 可以通过插入新的 XML 元素、删除现有 XML 元素、向 XML 元素插入属性、从 XML 元素删除属性以及许多其他类似操作来操作 XML 内容文件。

## 相对路径（XML 路径语言（XPath）//（双斜杠））在 Instruction Files 的情况下是否有效？
不，这不起作用，并且是 Instruction Files 的一个已知限制。

## Web Publisher 中的 Automatic Property Extraction (APE) 功能是什么意思？
Web Publisher 中的 Automatic Property Extraction (APE) 是一项功能，允许您根据为内容属性提供的值自动设置内容 XML 文件中指定 XML 元素的值，反之亦然。

## Automatic Property Extraction 在 Web Publisher 中适用于哪些特定小部件？
Web Publisher 中受 Automatic Property Extraction (APE) 支持的小部件如下：Textline、checkbox 和 Choice。

## Documentum 用于管理 Automatic Property Extraction (APE) 的对象类型有哪些？
Documentum 在 Web Publisher 中使用以下对象类型来管理 Automatic Property Extraction (APE)：dm_xml_application（dm_folder 的子类型）和 dm_xml_config（dm_document 的子类型）。

## Documentum 中 Workflow 流程模板和 Workflow 有什么区别？
Workflow 流程模板（dm_process 对象）包含工作流流程的定义，其中包含有关各种活动及其执行者、工作流流程中执行的操作等方面的规范，等等。

另一方面，Workflows（dm_workflow 对象）是从 Workflow 流程模板创建的用于路由内容的运行时实例。

## Documentum Workflows 中的 flows 是什么？
Flows 连接工作流模板中的各个活动，可以将路由对象向前（在批准的情况下）或向后（在拒绝的情况下）传递。

## 请说明 Workflow 活动的两种类型。
Workflows 中的活动有两种类型：Manual 和 Automatic。

Manual 活动由指定的组或用户手动执行，而 Automatic 活动由服务器自动执行。

## 关于 Workflows，packages 是什么？
Packages 添加到连接工作流模板中各个活动的 flows 中。这些进而路由工作流中的对象。

## 请说明 Site Caching Services (SCS) 中的不同类型发布操作。
Site Caching Services (SCS) 中的不同类型发布操作有：full refresh、incremental refresh、force refresh 和 single object publish。

## SCS 的各种发布参数（如要发布的对象、源发布文件夹、导出目录、目标主机等）在何处指定？
SCS 的各种发布参数在 Site Publishing Configuration 中指定，可使用 Documentum Administrator 轻松创建。

## Documentum 用于内部管理在 Documentum Administrator 中创建的 Site Publishing Configuration 的对象有哪些？
为了内部管理在 Documentum Administrator 中创建的 Site Publishing Configuration，Documentum 使用以下对象：dm_webc_config（用于源参数）和 dm_webc_target（用于目标参数）。

## 可以使用 Site Publishing Configuration 为已发布文档中的对象属性添加 HTML 元标签吗？
可以。您可以通过在 Site Publishing Configuration 的 Advanced 选项卡中选择 Add properties as HTML metatags 复选框，为已发布文档中的对象属性添加 HTML 元标签。

## 当数据发布到目标数据库时，SCS 创建了哪些表？
SCS 在发布操作期间在目标数据库上创建以下表：

_s 表（用于存储单值属性）、_r 表（用于存储多值属性）、_l 表（用于存储关系对象的相关信息（如果指定））和 _m 表（用于存储已发布文档的状态信息）。

## Site Publishing Configuration 中为 SCS 指定的 Export Directory 是什么？
Site Publishing Configuration 中指定的 Export Directory 是 Content Server 主机上的一个目录，Documentum SCS 将可发布文档连同其属性分别以内容文件和 properties.xml 文件（包含属性信息）的形式导出到该目录。

## 关于 SCS 发布操作，Online Synchronization 是什么意思？
Online Synchronization 在网站以周期性间隔进行批量发布而不是单项发布的情况下很有用。

对于正在运行的网站，批量发布可能会导致网站出现损坏的部分/链接。

使用 Online Synchronization，您需要在 Site Publishing Configuration 中在目标主机上指定一个 Online Synchronization 目录。

SCS 导出数据集（即已发布的内容文件 + 属性）首先放置在目标主机上的在线同步目录中，然后切换到目标根目录（webroot）。实际上，webroot 目录和在线同步目录会被重命名以进行同步。这对正在运行的网站的影响最小，并在很大程度上减少了不一致性。

## Site Publishing Configuration 中指定的 pre- 和 post-synch 脚本是什么？
可以编写并在 Site Publishing Configuration 中指定 pre- 和 post-synch 脚本，以便在 SCS 发布更新网站之前或之后执行自定义任务。请注意，这些脚本分别在 SCS 更新网站之前和之后执行。
