## 5 保存和加载配置

欢迎来到第 5 章！在上一章中，我们为应用程序添加了单独的视图模式。在此仅查看模式下，用户界面和选择功能被禁用。然后，我们添加了实例的简化版，最终是完整版的撤销/重做。对实例的设置更改现在可以被撤销或重新应用。

在本章中，我们将添加将应用程序配置保存到文件的能力。首先，我们将探索存储数据的不同文件类型。在考虑每种文件类型的优缺点并确定合适的格式后，我们将深入研究文件格式的结构。然后，我们将实现一个解析器类，允许我们加载和保存配置。最后，我们将在应用程序启动时加载默认配置，允许用户随意使用应用程序。

对于任何大型应用程序，能够保存创建或更改的数据的当前状态、停止应用程序并重新加载数据以继续工作至关重要。能够通过恢复最新保存的版本从挂起或崩溃的应用程序中恢复也至关重要。你不希望因为应用程序无法正确地将数据保存到本地或远程存储而冒着丢失数小时工作的风险。

在本章中，我们将涵盖以下主题：

- 文本或二进制文件格式——优缺点
- 选择保存数据的文本格式
- 探索 YAML 文件的结构
- 添加 YAML 解析器
- 保存和加载配置文件
- 在启动时加载默认配置文件

### 技术要求

本章的示例源代码位于 chapter05 文件夹中，OpenGL 的 01_opengl1_load_save 子文件夹和 Vulkan 的 02_vulkan_load_save。

在为应用程序添加保存和加载功能之前，让我们先看看一些将数据保存到存储设备的方法。

### 文本或二进制文件格式——优缺点

无论你玩游戏、剪辑视频、编辑照片，还是写文字或书籍，你都会使用所用软件集成的加载和保存功能。在安全位置保存游戏、存储大量编辑后的视频序列，或在文本编辑器中不时按下 Ctrl + S 已成为我们的常态。

但你有没有想过保存和加载的数据？你可能会对以下功能产生一些疑问，例如：

- 需要存储什么才能完全恢复最新状态？
- 应使用什么格式保存数据？
- 如果程序更新了，我还能加载已保存的数据吗？
- 我可以在没有原始程序的情况下读取或更改已保存的文件吗？

对于你只使用的所有程序，格式和数据量的决定权在于其开发人员。但对于我们的应用程序，我们必须决定需要保存哪些数据，以及如何将数据保存到存储中。

所有类型的数据格式都有优缺点；以下是简要概述。

### 保存和加载二进制数据

在计算机时代早期，存储空间和计算时间昂贵、珍贵且稀缺。为了在保存和恢复过程中最小化空间和时间，数据或多或少只是内存转储。

要保存的数据以内部数据类型存储在计算机的内存区域中，然后逐字节 literally 复制到软盘或硬盘上。




加载相同的数据与保存相同地简单快速：从存储设备读取数据到计算机内存（再次按字节）并将其解释为保存时使用的相同内部数据类型。

让我们来看看二进制数据的一些优缺点：

- **优点：**
  - 文件较小。
  - 保存和加载数据只需复制数据，从而实现更高的保存和加载操作速度。
- **缺点：**
  - 要更改应用程序外部的数据，需要特殊的知识。
  - 损坏的数据可能会导致不可预测的副作用或崩溃。
  - 保存文件格式的更新可能很困难。*魔法字节* 需要用于查找实际版本并将加载的数据映射到正确的内部数据类型。

由于不同架构的字节序（endianness），二进制数据可能在不同架构间不可移植。因此，通常的建议是除非绝对必要，否则避免使用二进制保存文件。我们仍然可以读取30年前在MS-DOS系统上创建的CONFIG.SYS和AUTOEXEC.BAT文件，但同时代电子表格计算器或文字处理器的二进制保存文件至少在没有正确工具或反向工程文件格式的艰苦工作下是不可用的。在像图片或声音文件等有良好文档记录和标准化的格式之外，保存二进制数据会引起麻烦，因为你可能无法在不同操作系统上打开二进制文件，甚至是同一系统的新版本。同样，在不同CPU架构、位宽或字节序上加载文件很可能会因数据类型的内存布局不同而失败。

由于CPU时间和存储空间不再受限，文本格式的优势现在明显超过二进制保存的优势。

## 保存和加载文本数据

随着CPU性能、网络带宽和存储空间的不断增加，文本格式开始成为保存数据的首选。当你可以使用简单的文本编辑器创建或修改保存的文件，或者通过将文本行打印到日志文件来查找错误时，开发代码会容易得多。

对于基于文本的保存文件，其条件与二进制保存不同：

- 文件较大，除非它们被压缩成.zip文件或类似文件。然后，需要另一个转换步骤（打包/解包）。
- 每次加载和保存数据时，必须将数据从二进制表示转换为文本并转换回来。
- 对于较大的更改，可能需要文件格式的领域特定知识，以从头创建保存文件。但对于简单的值更改，文本编辑器是我们唯一需要的工具。
- 损坏的数据可以修复，或者可以将损坏的数据元素简单地从文本文件中删除。最好只丢失部分数据，而不是全部。
- 文件格式更新可以通过在文件中增加版本号来检测，帮助应用程序使用正确的转换。
- 在具有不同架构或操作系统的计算机上加载相同文件根本没有问题。文本表示是相同的，由于从文本到二进制数据类型的转换，字节序或数据类型长度都不重要。
- 跨平台使用仍存在一些注意事项，如Windows和Linux中的不同路径分隔符，或系统区域设置中对点和逗号的不同解释。
- 如果配置需要拆分为多个文件，将所有文件打包成压缩文件是最常见的方式。通过将所有配置文件添加到.zip或.tar.gz文件中，你最终只得到一个文件，并由于压缩节省了一些磁盘空间。

使用文本表示来保存我们的保存文件是正确的做法。而且，通过定义一个简单的文件格式，甚至可以手动创建配置文件。

但是，在我们开始自行创建文件格式之前，让我们检查一些可用的文件格式。通过使用众所周知的文件格式，我们可以节省大量时间，因为我们不必创建解析和写入文件的函数。

## 选择文本格式来保存我们的数据

在本节中，我们将查看三种流行的配置文件格式：

- INI
- JSON
- YAML

这三种格式在某些领域有应用，但在其他领域可能不合适。

### INI文件格式

存储配置数据的最古老格式之一是所谓的INI格式。该名称来自文件扩展名.ini，它是initialization的三字母缩写。INI文件主要用于配置程序。

在INI文件中，简单的键/值对被存储并组织在可选部分中。一个部分名称用方括号括起来，部分范围从部分的开始到另一个部分的开始，或文件的末尾。

以下是一个数据库连接的部分和一些键/值对的示例：

```
[database-config]
type = mysql
host = mysql
port = 3306
user = app1
password = ASafePassword1#
```

部分可以通过用特殊字符（如点(.)或反斜杠(\)）分隔部分和子部分来创建某种层次结构。解析器识别这些部分划分至关重要；否则，每个部分将被独立处理，而不考虑它们的层次关系。

缺少重复的键名使得难以存储层次结构或非平凡数据，其中同一个键可能出现多次，如模型文件或实例配置。

### JSON文件格式

JSON，全称JavaScript Object Notation，在2000年代初首次亮相。就像在INI文件中一样，键/值对存储在JSON文件中。与INI文件部分类似的节不存在；相反，JSON文件允许创建复杂的树状层次结构。此外，还可以定义相同数据类型的数组。

JSON文件的主要用途是电子数据交换，例如在Web应用程序和后端服务器之间。文件格式对人类的可读性只是副作用；JSON文件主要由应用程序读取和写入。

以下是一个JSON文件示例，包含与INI文件示例相同的数据：

```
{
  "database-config": {
    "type": "mysql",
    "host": "mysql",
    "port": 3306,
    "user": "app1",
    "password": "ASafePassword1#"
  }
}
```

遗憾的是，由于大量的花括号，文件格式很难在第一次尝试时正确写入。此外，不允许使用注释，所以在飞行中测试不同选项只能通过保存原始文件的副本并调整内容来完成。

### YAML文件格式

YAML的名称最初是*Yet Another Markup Language*的缩写。在2000年代初，产品名称的*yet another*前缀被用作计算机相关幽默，指出未原创想法的不断重复。但是，由于YAML不是像HTML或XML那样的标记语言，名称的含义被更改为递归缩写*YAML Ain't Markup Language.*

YAML和JSON密切相关。一个JSON文件可以转换为YAML文件，反之亦然。两种格式之间的主要区别在于，YAML不使用花括号，而是通过缩进来创建层次结构。

YAML的主要目标是让人类可读。YAML格式被广泛用于创建和维护结构化和层次化的配置文件（即在配置管理系统和云环境中）。

以下是YAML文件格式的示例，再次使用与INI和JSON相同的数据：

```
database-config:
  type: mysql
  host: mysql
  port: 3306
  user: app1
  password: ASafePassword1#
```

由于YAML格式简单强大，具有我们需要的所有功能，并且可以读取和写入而不会在缺少花括号时绊倒，就像在JSON中一样，我们将使用YAML文件来存储我们应用程序的配置数据。

## 探索YAML文件的结构

让我们看看YAML文件的三种主要组件：

- 节点
- 映射
- 序列

让我们从节点开始。

### YAML节点

YAML文件的主要对象是一个所谓的节点。节点表示其下方的数据结构，可以是标量值、映射或序列。

我们使用此配置文件片段作为示例：

```
database-config:
  type: mysql
```

database-config和type都是YAML节点。虽然database-config节点包含包含键type和值mysql的映射，但type节点只包含标量值mysql。

### YAML映射

YAML映射包含零到任意数量的键/值对。此外，节点和映射之间存在有趣的相关性：键和值可以是另一个节点，在文件中创建层次结构。

让我们扩展之前节点部分中的配置文件片段：

```
database-config:
  type: mysql
  host: mysql
  port: 3306
```




正如前面所述，database-config 既是一个节点也是一个映射。该映射的键名为 database-config，其值是另一个映射，包含三个键/值对。

## YAML 序列

为了枚举相似的元素，会使用 YAML 序列。序列可以看作是 C++ std::vector，其中所有元素必须为同一类型。像 C++ vector 一样，你可以遍历序列，一次读取一个数据元素。

序列有两种不同的形式：块样式和流样式。

在块样式中，缩进的破折号 (-) 用作元素的指示符：

```
colors:
  - red
  - green
  - blue
```

相比之下，流样式使用方括号，元素之间用逗号分隔：

```
colors: [red, green, blue]
```

两种样式表示相同的数据。这取决于个人喜好和可读性。

通过组合映射和序列，可以创建复杂的数据结构。

## 映射和序列的组合

通过混合映射和序列，可以实现一种强大的数据表示方式。例如，我们可以像这样存储所有模型实例的位置和旋转：

```
instances:
  - position: [0, 0, 0]
    rotation: [0, 0, 0]
  - position: [1, 0, -3]
    rotation: [0, 90, 0]
  - position: [3, 0, 3]
    rotation: [0, -90, 0]
```

这里同时使用了映射和序列的组合。

首先，我们使用键 position 和 rotation 创建一个映射；值是数字的流式序列，表示一个 glm::vec3。YAML 总是存储标量数字的最短可能表示。因此，只要值没有小数部分，就会使用整数值，即使对于 float 和 double 类型也是如此。然后，position 和 rotation 的映射被用于块序列中，以创建模型实例的数组式表示。

要将实例数据读入我们的应用程序，我们必须首先遍历模型实例序列，并对每个实例提取 position 和 rotation 值。

在对 YAML 文件格式进行基本介绍之后，我们现在将为我们的应用程序实现一个 YAML 解析器和写入器类，以便保存和加载其配置。将配置存储在磁盘上就像保存文本文档一样——我们可以退出应用程序并稍后继续在虚拟世界中工作。我们也可以使用保存的文件返回到虚拟世界的先前状态。

## 添加 YAML 解析器

像我们已经在使用的其他工具一样（Open Asset Import Library、GLFW 或 ImGui），我们将使用一个免费的开源解决方案：yaml-cpp。

通过集成 yaml-cpp，我们可以用最少的精力从 C++ 读取和写入 YAML 文件。最大的步骤是确保我们的自定义数据类型为 yaml-cpp 所知。此外，我们必须考虑数据文件的适当结构。

让我们开始探索如何将 yaml-cpp 集成到我们的项目中。

## 获取 yaml-cpp

对于 Linux 系统，获取 yaml-cpp 很简单。与其他工具类似，大多数发行版已包含 yaml-cpp 库和头文件。例如，在 Ubuntu 22.04 或更高版本中，可以使用以下命令安装 yaml-cpp 及其开发文件：

```
sudo apt install libyaml-cpp-dev
```

如果你使用的是基于 Arch 的 Linux 发行版，可以使用以下命令安装 yaml-cpp：

```
sudo pacman -S yaml-cpp
```

对于 Windows，我们也很幸运。yaml-cpp 使用 CMake，并且通过使用 CMake FetchContent 命令，只需几行代码即可将下载 yaml-cpp 添加到项目中。首先，我们将 FetchContent 声明添加到项目根目录的 CMakeLists.txt 文件中。我们使用的是 yaml-cpp 的 0.8.0 版本：

```
FetchContent_Declare(
    yaml-cpp
    GIT_REPOSITORY https://github.com/jbeder/yaml-cpp
    GIT_TAG 0.8.0
)
```

确保我们在 CMakeLists.txt 文件中的 WIN32 部分内。我们不需要在 Linux 上下载该库。

然后，我们触发 yaml-cpp 的下载并添加目录的变量：

```
FetchContent_MakeAvailable(yaml-cpp)
FetchContent_GetProperties(yaml-cpp)
if(NOT yaml-cpp_POPULATED)
    FetchContent_Populate(yaml-cpp)
    add_subdirectory(${yaml-cpp_SOURCE_DIR}
        ${yaml-cpp_BINARY_DIR} EXCLUDE_FROM_ALL)
endif()
```

Windows 还需要一个脚本来检测下载的依赖项。检测脚本必须命名为 Findyaml-cpp.cmake 并放置在 cmake 文件夹中。

脚本的主要功能归结为这两个 CMake 函数：

```
find_path(YAML-CPP_INCLUDE_DIR yaml-cpp/yaml.h
    PATHS ${YAML-CPP_DIR}/include/)
find_library(YAML-CPP_LIBRARY
    NAMES ${YAML-CPP_STATIC} yaml-cpp
    PATHS ${YAML-CPP_DIR}/lib)
```

由于 CMake 的 FetchContent，YAML-CPP_DIR 变量会被填充为下载的 yaml-cpp 代码的路径。因此，脚本只需检查头文件和库是否可以找到。

## 将 yaml-cpp 集成到 CMake 构建中

对于 Linux 和 Windows，我们必须为编译器设置正确的包含路径，并且必须将 yaml-cpp 库添加到链接库列表中。

要更新包含路径，将 YAML_CPP_INCLUDE_DIR 变量添加到 include_directories 指令中：

```
include_directories(... ${YAML_CPP_INCLUDE_DIR})
```

对于链接器，在 Windows 中将 yaml-cpp::yaml-cpp 添加到 target_link_libraries 中：

```
target_link_libraries(... yaml-cpp::yaml-cpp)
```

对于 Linux，只需要共享库的名称 yaml-cpp：

```
target_link_libraries(... yaml-cpp)
```

再次运行 CMake 后，yaml-cpp 将被下载并可供其余代码使用。

## 添加解析器类

解析 YAML 文件以进行加载以及为写入创建内容将在 tools 目录中的一个新类 YamlParser 中完成。我们可以在代码中使用 yaml-cpp，在包含头文件后：

```
#include <yaml-cpp/yaml.h>
```

为了在加载时存储中间数据或创建要保存到磁盘的数据结构，需要两个额外的私有成员：

```
YAML::Node mYamlNode{};
YAML::Emitter mYamlEmit{};
```

YAML::Node 将磁盘上 YAML 文件的节点转换为 C++ 数据结构，简化了对已加载数据的访问。YAML::Emitter 用于通过追加数据元素在内存中创建 YAML 文件，最终将结构化数据写入文件。

## 使用 yaml-cpp 的节点类型

访问 yaml-cpp 节点的结构化或标量数据是通过使用节点名称作为 YAML::Node 类中存储的 C++ 映射的索引来完成的：

```
mYamlNode = YAML::LoadFile(fileName);
YAML::Node settingsNode = mYamlNode["settings"];
```

要检索简单键/值映射的标量值，使用特殊的 as 操作符：

```
int value = dataNode.as<int>();
```

由于 YAML 文件不知道值中存储的数据类型，我们必须明确告诉 yaml-cpp 如何解释传入的数据。在这里，yaml-cpp 将尝试以 int 的形式获取 dataNode 节点的值。

在定义转换模板后，自定义数据类型如结构体也可以直接读入相同类型的变量中：

```
InstanceSetting instSet = instNode.as<InstanceSettings>();
```

我们将在 保存和加载配置文件 部分处理转换模板。

## 访问序列和映射

在 yaml-cpp 中，序列可以通过使用 for 循环迭代并通过索引访问元素来读取：

```
for (size_t i = 0; i < instNode.size(); ++i) {
    instSettings.emplace_back(
        instNode[i].as<InstanceSettings>());
}
```

序列的元素以它们在 YAML 文件中出现的相同顺序提供。

对于映射，需要使用迭代器样式的 for 循环：

```
for(auto it = settingsNode.begin();
    it != settingsNode.end(); ++it) {
    if (it->first.as<std::string>() == "selected-model") {
        ...
    }
}
```

映射元素的键可以通过 C++ 映射容器的 first 访问器读取。同样，我们必须告诉 yaml-cpp 映射键的数据类型是什么。然后，可以使用 C++ 映射容器的第二个访问器检索值。

如果读取值失败，例如类型错误或不存在这样的节点，将抛出异常。为了避免程序被终止，我们必须处理所有抛出的异常。





## 处理 yaml-cpp 抛出的异常

与返回错误代码不同，yaml-cpp 在出现问题时总是抛出异常。默认情况下，任何未处理的异常都会终止程序。在 C++ 中处理异常的方式与其它语言类似：

```
try {
  mYamlNode = YAML::LoadFile(fileName);
} catch(...) {
  return false;
}
```

可能导致异常的调用会被包含在一个 try 块中，如果发生异常，则 catch 块会被执行。

我们只需捕获所有异常，因为任何解析失败都可能导致空的或不完整的配置文件。如果你想要更详细的异常处理，可以查阅 yaml-cpp 的源代码。

掌握了 yaml-cpp 的基础知识后，我们就可以开始实现保存和加载 YAML 文件的代码。我们将从保存功能开始，因为在磁盘上创建文件后，将数据元素重新加载回应用程序会比先手工制作配置文件容易得多。

## 保存和加载配置文件

构建我们的配置文件首先要决定需要存储什么以及如何存储这些元素。通过重用我们的自定义数据类型（如 InstanceSettings），创建保存和加载文件的函数可以被简化。现在我们不再需要逐个读取每个值，而是可以使用 AssimpInstance 类中的 getInstanceSettings() 和 setInstanceSettings() 调用，直接在解析器和实例之间传输值。

我们将首先探索想要保存的内容，并在添加将自定义数据写入文件的代码后，会添加一个用户界面对话框，以便以简单的方式将文件保存到磁盘。最后，我们将逐步介绍将配置重新加载回应用程序的过程。

## 重载 emitter 的输出运算符

yaml-cpp 的创建者添加了一个很方便的方法，可以将复杂结构的内容输出到 YAML::Emitter。我们只需要在 tools 文件夹中的 YamlParser.cpp 文件里重载 operator<< 即可：

```
YAML::Emitter& operator<<(YAML::Emitter& out,
    const glm::vec3& vec) {
    out << YAML::Flow;
    out << YAML::BeginSeq;
    out << vec.x << vec.y << vec.z;
    out << YAML::EndSeq;
    return out;
}
```

对于 glm::vec3 数据类型，我们添加一个 flow 类型序列，然后向流中添加向量的三个元素。在最终文件中，会出现一个默认的 YAML 序列，其中包含 glm::vec3 向量的值，如此示例所示：

```
[1, 2, 3]
```

我们的 InstanceSettings 结构体必须作为 YAML map 的 key/value 对添加。map 的开始和结束在存储 InstanceSettings 的函数中设置：

```
YAML::Emitter& operator<<(YAML::Emitter& out,
    const InstanceSettings& settings) {
    out << YAML::Key << "model-file";
    out << YAML::Value << settings.isModelFile;
    out << YAML::Key << "position";
    out << YAML::Value << settings.isWorldPosition;
    out << YAML::Key << "rotation";
    out << YAML::Value << settings.isWorldRotation
    ...
}
```

生成的 map 将作为 YAML 序列的值添加，从而在配置文件中存储所有相关的实例数据。

## 创建和写入配置文件

要创建配置文件，渲染器将实例化我们 YamlParser 类的本地对象，并在其上调用 createConfigFile()：

```
YamlParser parser;
if (!parser.createConfigFile(mRenderData, mModelInstData)) {
    return false;
}
```

在 createConfigFile() 方法中，YAML::Emitter 将被填充我们的数据结构。作为示例，我们将在文件顶部添加一条注释，并在第二行保存版本号：

```
mYamlEmit << YAML::Comment("Application viewer
    config file");
mYamlEmit << YAML::BeginMap;
mYamlEmit << YAML::Key << "version";
mYamlEmit << YAML::Value << 1.0f;
mYamlEmit << YAML::EndMap;
...
```

在最终的 YAML 文件中，以下首行将会出现：

```
# Application viewer config file
version: 1
```

作为另一个示例，要存储实例设置，我们创建一个名为 instances 的 map，并开始创建序列作为其值：

```
mYamlEmit << YAML::BeginMap;
mYamlEmit << YAML::Key << "instances";
mYamlEmit << YAML::Value;
mYamlEmit << YAML::BeginSeq;
```

然后，我们可以对实例进行 for 循环，并使用实例的 getInstanceSettings() 调用直接将实例设置存储到 emitter 流中：

```
for (const auto& instance : modInstData.miAssimpInstances) {
    mYamlEmit << YAML::BeginMap;
    mYamlEmit << instance->getInstanceSettings();
```

```
mYamlEmit << YAML::EndMap;
}
```

得益于 operator<< 的重载，循环内部不需要复杂的处理。

作为最后一步，我们关闭实例设置的序列，并关闭 instances key 的 map：

```
mYamlEmit << YAML::EndSeq;
mYamlEmit << YAML::EndMap
```

最终的 YAML 文件将包含所有实例的序列，包括新添加的模型文件名：

```
instances:
- model-file: Woman.gltf
  position: [0, 0, 0]
  rotation: [0, 0, 0]
  scale: 1
  swap-axes: false
  anim-clip-number: 0
  anim-clip-speed: 1
```

如果我们想在进行任何磁盘写入之前查看创建的配置文件的內容，可以创建一个 C 字符串并通过 logger 类输出该字符串：

```
Logger::log(1, "%s\n", mYamlEmit.c_str());
```

将文件写入磁盘将使用 std::ostream。以下列表中为简洁起见省略了流的错误处理，但保存文件到磁盘本质上只需三行代码：

```
bool YamlParser::writeYamlFile(std::string fileName) {
    std::ofstream fileToWrite(fileName);
    fileToWrite << mYamlEmit.c_str();
    fileToWrite.close();
    return true;
}
```

首先，我们使用给定的文件名创建输出流。然后，我们将 YAML::Emitter 的 std::string 转换为 C 字符串并写入输出流。通过关闭流，文件将被刷新到存储设备。

## 在用户界面中添加文件对话框

为了允许用户将配置文件存储在任意位置并使用自定义名称，我们将在用户界面中添加一个文件对话框。我们已经在使用基于 ImGui 的文件对话框来加载模型文件，并且可以重用同一个对话框实例向用户展示保存文件对话框。

要创建让用户选择文件名和位置的对话框，必须对名为 config 的 IGFD::FileDialogConfig 变量做三处更改。

首先，通过选择现有文件，我们需要一个额外的对话框来确认是否覆盖文件。幸运的是，文件对话框已内置了这样的确认对话框。我们需要做的只是添加标志 ImGuiFileDialogFlags_ConfirmOverwrite：

```
config.flags = ImGuiFileDialogFlags_Modal |
    ImGuiFileDialogFlags_ConfirmOverwrite;
```

如果我们选择一个现有文件，将会显示一个新对话框，询问用户是否要替换现有文件。

接下来，我们将为配置提供默认路径和文件名：

```
const std::string defaultFileName = "config/conf.acfg";
config.filePathName = defaultFileName.c_str();
```

这里，我们使用 config 文件夹和一个名为 config.acfg 的文件来向用户展示默认文件。文件对话框代码将自动进入 config 文件夹并填入文件名和扩展名。

作为最后一步，我们将 .acfg 添加为对话框中唯一的扩展名：

```
ImGuiFileDialog::Instance()->OpenDialog(
    "SaveConfigFile", "Save Configuration File",
    ".acfg", config);
```

通过为配置文件使用新的扩展名，我们避免了麻烦，例如尝试加载不同文件格式或覆盖系统上的其它文件。

文件对话框的 OK 按钮会检索所选文件名，并调用负责将配置保存到磁盘的回调方法：

```
if (ImGuiFileDialog::Instance()->IsOk()) {
    std::string filePathName =

```
ImGuiFileDialog::Instance()->GetFilePathName();
saveSuccessful =
    modInstData.miSaveConfigCallbackFunction(
        filePathName);
}
```

我们将回调函数的结果存储在布尔变量 `saveSuccessful` 中。这样，我们可以检查是否存在错误，并在配置保存失败时向用户显示对话框。

为了告知用户保存错误，我们仅实现了一个简单对话框，通知用户查看应用程序的输出消息，以了解写入错误原因的详细信息。

如果你现在加载一些模型、创建实例或克隆，并保存配置，可以检查生成的配置文件。“决定配置文件中存储什么”一节中的所有数据都应该出现在配置文件中。

将数据保存到磁盘只是完成了一半工作。为了从上次保存文件的位置继续工作，我们需要将配置文件重新加载到应用程序中。

## 重新加载配置文件并解析节点

为了支持解析 YAML 文件中的自定义数据类型，yaml-cpp 允许我们为位于 YAML 命名空间中名为 `convert` 的结构体定义 C++ 模板。`convert` 结构体必须实现名为 `encode` 和 `decode` 的两个方法，用于将 C++ 类型序列化为 YAML（`encode`）以及从 YAML 反序列化回 C++（`decode`）。通过这两个方法，yaml-cpp 实现了 C++ 类型与 YAML 条目之间的无缝转换。

`encode` 方法从原始或自定义数据类型创建新的 YAML 节点，而 `decode` 方法读取 YAML 节点数据并返回原始或自定义数据类型。

为了将 `glm::vec3` 元素写入 YAML 节点，并将 YAML 节点读回为 `glm::vec3`，需要在头文件中实现以下模板代码：

```
namespace YAML {
    template<>
    struct convert<glm::vec3> {
        static Node encode(const glm::vec3& rhs) {
            Node node;
            node.push_back(rhs.x);
            node.push_back(rhs.y);
            node.push_back(rhs.z);
            return node;
        }
    };
}
```

为了保存 `glm::vec3` 的数据，我们创建一个名为 `node` 的新 YAML 节点，并将 `glm::vec3` 的 `x`、`y`、`z` 三个元素添加到该节点。然后将该节点返回给 `encode()` 方法的调用者。

使用 `decode()` 方法将节点中的数据读回 `glm::vec3` 变量：

```
static bool decode(const Node& node, glm::vec3& rhs) {
    if (!node.IsSequence() || node.size() != 3) {
        return false;
    }
```

检查节点类型和大小是否正确是可选的，但为确保自定义数据类型的数据正确，防止运行时错误，这是一种良好的风格。跳过此检查并尝试解析错误的数据类型将导致异常，如果未处理，将终止整个程序。

然后，我们通过序列索引从节点读取数据，并将 `glm::vec3` 的 `x`、`y`、`z` 三个元素设置为节点中的浮点值：

```
rhs.x = node[0].as<float>();
rhs.y = node[1].as<float>();
rhs.z = node[2].as<float>();
return true;
};
```

定义 `encode()` 和 `decode()` 方法后，我们可以通过常规赋值在 YAML 节点和 `glm::vec3` 之间交换数据：

```
glm::vec3 data;
node["rotation"] = data.isWorldRotation;
data.isWorldRotation = node["rotation"].as<glm::vec3>();
```

`InstanceSettings` 结构体也实现了同样的两个方法，帮助我们直接将实例设置读回为 `InstanceSettings` 类型的变量。为了避免污染解析器类的头文件，我们在 `tools` 文件夹中创建了一个名为 `YamlParserTypes.h` 的新头文件。`YamlParserTypes.h` 头文件将被包含在 `YamlParser` 类的头文件中，以使新的转换可用。

一旦配置文件被成功解析，所有设置、模型路径和实例设置都会被提取出来。但在加载模型和创建新实例之前，我们必须首先清除当前的模型和实例列表。

## 清理并根据保存的值重新创建场景

移除所有模型和实例是一个简单直接的过程。在渲染器中，我们必须执行以下步骤以获得全新的环境：

- 1. 将包含当前选中实例和模型的 `miSelectedInstance` 和 `miSelectedModel` 设为零。此步骤以及步骤 2 和 3 中的变量在第一章的“动态模型和实例管理”一节中介绍过。然后，将在索引零处创建新的空模型和空实例。
- 2. 擦除 `miAssimpInstances` 向量并清空 `miAssimpInstancesPerModel` 映射。现在，所有模型都未被使用。
- 3. 擦除 `miModelList` 向量。由于所有实例已被删除，模型的共享指针将不再被引用，模型将被删除。
- 4. 添加新的空模型和空实例。空模型和空实例都必须是模型列表、实例向量和映射的第一个元素。
- 5. 清空撤销和重做堆栈。在堆栈中，我们只使用了弱指针，因此此步骤可以随时执行。
- 6. 更新三角形计数。移除所有模型和实例后，三角形计数应为零。

清理所有模型和实例的整个过程已添加到渲染器类的新方法 `removeAllModelsAndInstances()` 中，无论何时我们需要干净的全新环境，都能简化操作。

现在，我们可以从磁盘加载模型文件，但不创建默认实例。加载所有模型后，我们在模型列表中搜索 `InstanceSettings` 中的模型，创建新实例，并应用配置文件中的设置。

接下来，我们应该枚举实例，因为实例索引号没有存储在 `InstanceSettings` 中。但实例应保持在保存时所处的相同索引，这是由于在创建 YAML 发射器时对 `miAssimpInstances` 向量进行线性读取，以及在解析 YAML 文件时对节点进行相同的线性读取。

最后，我们从解析器恢复相机设置、选中的模型和实例，以及选中高亮的状态。

此时，配置应已完全加载，应用程序应包含与保存操作时相同的模型、实例和设置。

加载过程还有一个问题：如果配置文件解析只部分失败，我们该怎么办？也许模型文件被重命名或删除，或者文件被截断或损坏，最后一个实例的设置不完整。

## 严格或宽松的配置文件加载

一种克服解析错误的方法是在删除应用程序当前所有内容之前丢弃整个配置。这种严格加载类型易于实现；任何解析错误都会在解析时使配置文件失效。我们忽略加载请求，只向用户显示错误消息。

另一种选择是宽松解析。我们会尽力加载有效的模型，并用默认值填充缺失的配置部分，同时告知用户配置文件的某些部分无法加载。

在这两种情况下，错误消息都应给出解析失败的详细位置提示。因此，可以扩展异常处理，以确切了解出错的位置和原因。对于宽松处理，应尽可能向用户提供有关受影响模型、实例或设置的更多附加信息。

应用程序的创建者需要决定哪种策略最适合。通常，应尝试尽可能多地恢复数据。只丢失所创建作品的一小部分总比丢失所有数据要好。

## 导致文件损坏的常见错误

有几个因素可能导致已保存的配置文件损坏。以下列出一些常见原因：

- 写入文件时磁盘或分区空间已满：即使在今天存储空间充足的情况下，这种情况也可能发生，你只会保存部分数据。
- 权限问题：有时，你可能拥有创建文件的权限，但没有写入文件内容的权限。因此，你的文件似乎已保存，但文件的长度为零字节。
- 保存到远程位置时的连接错误：在写入较大文件时，连接可能会中断，导致文件只写入了一部分。




- 转换错误，例如通过电子邮件发送文件：邮件程序或邮件服务器可能会以错误的方式转换文件，导致文件部分损坏，部分字符被替换。
- 不兼容的区域设置：保存文件的机器可能使用逗号作为小数分隔符，而你的电脑使用点作为小数分隔符。文件中的数字将被错误解释，甚至在解析失败时被设为零。这个问题很难发现，很容易被忽略。
- 编码错误，如版本处理错误、转换错误或不完整的错误/异常处理：你可能没有保存所有数据，意外地将数据转换为错误格式，或未能解析文件中的部分数据。你应该尽可能针对多种数据类型和对象组合测试读写文件的代码，以发现此类错误。
- 你应该意识到你的保存文件可能在你的机器上或在传输过程中损坏。因此，经常保存你的工作，使用 Git 等版本控制系统来存储文件的不同版本，并定期备份所有配置文件。

现在我们有了保存和加载应用程序状态的代码，我们可以在应用程序启动时提供一个预定义的默认配置。

## 启动时加载默认配置文件

为了帮助用户探索新应用程序，除了详尽的教程之外，在首次启动时，或也许在应用程序的任何一次启动时，都可以加载一个使用该应用程序创建的内容的简单示例。通过摆弄可用的选项，我们可以帮助理解应用程序的工作原理，以及可能的内容操作。

启动时加载默认配置可以通过不同方式实现。配置文件可以在编译时添加到应用程序中（*烘焙*到应用程序中），或一个或多个示例文件可以放在可达的文件夹中，并在启动时加载示例文件。通常，应用程序中有一个单独的配置设置可用于禁用示例文件的自动加载。

作为示例，我们将在应用程序启动时从加载和保存对话框加载配置文件 `config/conf.acfg`。得益于已实现的 YAML 解析器和文件加载代码，对渲染器类的更改只需几行即可完成。

## 保存和加载配置

首先，我们将默认配置文件定义为渲染器类的一个新私有成员变量 mDefaultConfigFileName：

```
const std::string mDefaultConfigFileName =
"config/config.acfg";
```

通常应避免硬编码文件路径或文件名，但对于第一个配置文件，我们陷入了鸡生蛋问题。如果我们想将默认配置的名称存储在另一个配置文件中，而不是在代码中硬编码文件名，我们将需要另一个硬编码的文件名。这种引导问题只能通过硬编码第一个值来解决。

然后，在渲染器的 init() 方法中，我们尝试加载默认配置文件：

```
if (!loadConfigFile(mDefaultConfigFileName)) {
addNullModelAndInstance();
}
```

如果文件找不到或加载失败，我们只创建空模型和空实例。由于所有其他值在首次启动时都是默认值，我们最终得到的应用程序与根本没有默认配置的情况相同。

为应用程序实现加载和保存功能需要对正确的保存文件类型进行一些研究，以及将功能实际实现到现有代码中的更多工作。所有更改和新功能都应反映在应用程序的保存文件中，因此随着应用程序功能的更改，保持加载和保存代码更新需要更多工作。通过在配置文件中添加版本控制方案，我们甚至能够加载来自应用程序不同开发阶段的配置。

在本章中，我们添加了将应用程序当前配置保存到文件并将相同配置加载回应用程序的功能。首先，我们评估了二进制和文本保存文件的优缺点，并检查了三种常见的文本文件类型，以找到适合我们保存文件的格式。接下来，我们探索了所选的 YAML 文件格式并实现了保存和加载功能。最后，我们添加了一个在应用程序启动时加载的默认文件，以帮助用户进行应用程序处理的最初步骤。

在下一章中，我们将处理应用程序中的自定义摄像机。目前，我们仅使用 *内部* 摄像机在虚拟世界中飞行。通过添加自定义摄像机类型，可以为虚拟世界提供更多可视化选项。我们将添加一种第三人称风格的摄像机，像动作游戏一样跟随其中一个实例，以及一种跟随一个实例的固定摄像机。此外，还将添加一个简单的摄像机管理器，并且摄像机的配置也将保存在配置文件中。

## 实践环节

你可以对代码做一些补充：

- 添加一个菜单项来创建一个新的、空的场景。
- 目前，我们只能加载和保存配置文件。移除所有模型和实例仍需手动完成。添加一个菜单项及代码，一次性移除所有模型和实例，为用户提供从零开始的简单方式。
- 添加标志和确认对话框，如果设置被更改。
- 如果一个模型的设置被更改，设置一个脏标志以记住应用程序用户更改了加载的模型实例或保存的状态。然后，如果用户想加载另一个配置文件，重新开始使用空配置或退出应用程序并显示确认对话框，以确保有机会保存当前设置。
- 在标题中添加脏标记。
- 许多其他应用程序都会向用户展示某种通知，说明自上次保存以来已进行了更改。应用程序窗口的标题会根据我们处于编辑模式还是查看模式进行调整，因此在窗口标题中添加星号 (*)，或添加诸如“未保存”等字样应该很容易。

## 附加资源

以下是 yaml-cpp 的 GitHub 仓库：https://github.com/jbeder/yaml-cpp。

## 加入我们的 Discord 社区

加入我们的 Discord 社区，与作者和其他读者进行讨论：
https://packt.link/cppgameanimation

![](img/3cfb9832c943f5f7366fd104b5255971_182_0.png)
