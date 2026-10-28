

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

