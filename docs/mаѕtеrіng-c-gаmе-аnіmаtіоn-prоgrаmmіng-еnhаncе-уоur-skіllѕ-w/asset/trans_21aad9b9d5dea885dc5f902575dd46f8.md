

由于本书将使用开源软件和平台无关的库，因此你应该能够在 Windows 和 Linux 上“开箱即用”地编译和运行代码。你可以在下面的 *技术要求* 部分找到所需软件和库的详细清单，以及它们的安装说明。

## Technical requirements

本章你需要以下内容：

- 一台运行 Windows 或 Linux 的 PC，以及本节后面列出的工具
- 用于源代码管理的 Git
- 一个文本编辑器（如 Notepad++ 或 Kate）或完整的 IDE（如 Windows 版 Visual Studio 2022，或 Linux 版 Eclipse/KDevelop）

> Important note
> 
> 需要一个较新的 C++ 编译器来编译代码。在当前的 CMake 构建系统中，已配置 C++17，但代码已知可与更新的 C++ 标准一起使用，直至 C++26 包含在内（尽管编译器必须支持这些标准）。

现在，让我们获取本书的源代码并开始解压代码。

## Getting the source code and the basic tools

本书的代码托管在 GitHub 上，你可以在这里找到：

https://github.com/PacktPublishing/Mastering-Cpp-Game-Animation-Programming

你需要安装 Git，因为构建系统利用 Git 下载示例中使用的第三方项目。

在 Linux 系统上，使用你的包管理器。对于 Ubuntu，以下命令可安装 Git：

```
sudo apt install git
```

在 Windows 上，你可以从这里下载 Git：https://git-scm.com/downloads。

要解压代码，你可以使用以下两种方法中的任意一种。

### Getting the code using Git

要获取书中的代码，你应该使用 Git。使用 Git 可为你提供额外的功能，例如为你的更改创建本地分支、跟踪你的进度以及将你的更新与示例代码进行比较。此外，如果你在探索源代码或在每章末尾的实践环节中破坏了代码，你可以轻松地还原更改。

你可以通过 Git GUI、在 Visual Studio 2022 中克隆存储库，或在 CMD 中执行以下命令，将代码的本地检出放在系统上的特定位置：

```
git clone https://github.com/PacktPublishing/Mastering-Cpp-Game-Animation-Programming
```

请确保你使用的是没有空格或特殊字符（如变音符号）的路径，因为这可能会让某些编译器和开发环境感到困惑。

### Getting the code as a ZIP file

虽然推荐使用 Git，但你也可以从 GitHub 将代码作为 ZIP 文件下载。你需要将 ZIP 文件解压到系统上你选择的某个位置。此外，请确保解压 ZIP 文件的路径中不包含空格或特殊字符。

在我们使用本书的代码之前，必须安装一些工具和库。我们将先从 Windows 安装开始，然后是 Linux 安装。

### Installing the required tools and libraries for Windows

要在 Windows 机器上编译示例代码，我建议使用 Visual Studio 2022 作为 IDE，因为它包含了快速入门所需的一切。使用其他 IDE 如 Eclipse、Rider 或 KDevelop 也没问题，因为构建由 CMake 管理，但你可能需要安装 C++ 编译器，如 MSYS2 加上编译器包作为额外依赖。

### Installing Visual Studio 2022 on Windows

如果你想使用 Visual Studio 来使用示例文件且尚未安装，请从 https://visualstudio.microsoft.com/de/downloads/ 下载免费的 Community Edition。

双击该文件将自动启动 Windows 注册表编辑器，以将设置导入 Windows 注册表。确认 UAV 对话框和以下警告对话框后，单击“是”，注册表编辑器将导入新设置。

现在，重新启动 PC 以激活长路径名称并继续安装。

## Downloading Open Asset Import Library

对于 Windows，必须从源文件构建并安装 Open Asset Import Library。从 https://github.com/assimp/assimp 克隆存储库到新的 Visual Studio 2022 项目中，如图 1.3 所示：

![](img/3cfb9832c943f5f7366fd104b5255971_38_0.png)

作为替代方案，你可以从 Git Bash 或通过 Git GUI 创建克隆：

```
git clone https://github.com/assimp/assimp
```

## Configuring the build

我们需要进行一些调整以创建静态库而不是动态库。使用静态库使我们的构建过程更简单，因为我们不必担心额外的 DLL 文件。

要更改 CMake 设置，在 CMakeLists.txt 文件上右键单击后选择以下选项：

![](img/3cfb9832c943f5f7366fd104b5255971_38_1.png)

在出现的 Visual Studio 2022 的 **Configuration** 选项卡中，将配置名称更改为 x64-RelWithDebInfo，并将配置类型更改为 RelWithDebInfo：

![](img/3cfb9832c943f5f7366fd104b5255971_39_0.png)

Figure 1.5: Modifying the current configuration of the asset importer

通过使用 RelWithDebInfo，将创建带有调试信息的发布版本。生成的可执行文件将由编译器优化，但文件仍包含数据以允许在出现问题时调试程序。

接下来，更改 CMake 设置中的以下设置。你可以使用左下角名为 **Filter variables...** 的搜索字段来搜索指定的设置：

- 禁用构建共享库：

![](img/3cfb9832c943f5f7366fd104b5255971_39_1.png)

Figure 1.6: Switching the setting to create a static library

- 更改 C 运行时库的链接方式：

![](img/3cfb9832c943f5f7366fd104b5255971_39_2.png)

Figure 1.7: Linking the C runtime statically

- 删除库后缀以创建不带编译器版本的文件名：

![](img/3cfb9832c943f5f7366fd104b5255971_40_0.png)

Figure 1.8: Removing the suffix of the created file

接下来，选择 Build 然后在 CMakeLists.txt 文件的上下文菜单中选择 Install。

安装完成后，将生成以下文件夹结构：

![](img/3cfb9832c943f5f7366fd104b5255971_40_1.png)

Figure 1.9: Asset importer library and includes

我们必须使本节讨论的所有文件对本书中的所有示例都可用。为此，有两个选项可用——将文件复制到固定路径或添加环境变量。

## Copying the Assimp files

首先，在你的计算机上创建此文件夹：

```
C:\Program Files\assimp
```

然后，将两个文件夹 lib 和 include 复制到其中：

![](img/3cfb9832c943f5f7366fd104b5255971_40_2.png)

Figure 1.10: The two folders have been copied to the Program Files folder

## Compiling and starting the example code

运行示例可以通过两种不同的方式完成：按照书中示例逐个进行，或一次性编译所有代码以浏览所有示例。

编译代码可以通过以下步骤完成：

1. 要打开示例项目，请从 Visual Studio 2022 开始屏幕中选择 Open a local folder 或从 Visual Studio 2022 的 File 菜单中选择 Open CMake，然后导航到你想要编译的示例代码所在的文件夹，或导航到示例代码的顶级文件夹，如果你想一次性编译所有示例。Visual Studio 将自动检测并为你配置所选文件夹中的 CMake。输出窗口的最后一行应如下所示：

```
1> CMake generation finished.
```

2. 这确认了 CMake 文件生成的成功运行。

3. 现在，通过右键单击 CMakeLists.txt 文件设置启动项——此步骤是构建和运行项目所必需的：

![](img/3cfb9832c943f5f7366fd104b5255971_42_0.png)

4. 设置启动项后，我们可以构建当前项目。右键单击 CMakeLists.txt 文件并选择 Build：

![](img/3cfb9832c943f5f7366fd104b5255971_42_1.png)

