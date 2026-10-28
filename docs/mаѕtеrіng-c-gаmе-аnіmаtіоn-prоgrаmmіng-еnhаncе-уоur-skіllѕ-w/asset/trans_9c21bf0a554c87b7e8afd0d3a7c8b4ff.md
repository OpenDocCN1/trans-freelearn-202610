

## 环境音乐

与环境音类似，环境音乐有助于防止游戏中出现死寂。完全不播放任何声音可能是为了营造紧张感，但在游戏中连续数小时寂静穿行可能会变得无聊。添加一段风格和节奏与游戏玩法相匹配的音乐曲目，有助于玩家享受游戏中即便是缓慢的段落。

## 自适应音乐播放

当玩家在开放世界中进入不同区域，或进入风格不同的环境、另一房间时，更换音乐以匹配新地点是个好主意。进入房间时那个著名的“听到Boss音乐”的瞬间，再加上门锁上的声音，将为玩家创造难忘的时刻。

## 允许自定义音乐

一个有趣的音乐选择选项是让玩家将本地音乐添加到游戏播放的曲目列表中，甚至完全替换音乐。看到游戏角色一边探索虚拟世界，一边聆听一些喜爱的音乐曲目，对玩家来说可能是一个加分项。

为了体验音频回放所能带来的差异，我们将添加一个 C++ 类来管理音效和音乐回放。

## 实践：实现音频管理器

音频管理器类由两个不同的部分组成。一方面，我们需要一个音频管理器与渲染器及其他类之间的高级接口。另一方面，封装好的低级接口将音频管理器功能与操作系统的音频库连接起来，这样我们就可以用另一个变体替换音频库。

我们将先简要概述高级接口，然后再深入低层实现。

### 定义高级接口

新的 AudioManager 类由 AudioManager.h 头文件和 AudioManager.cpp 实现文件组成，将添加到 tools 文件夹中。我们将添加一个常规音乐/声音播放器应具备的基本功能：

- 初始化并清理音频库
- 从文件系统中的文件加载音乐曲目
- 从文件系统中的文件加载音效
- 播放和停止音乐与音效
- 暂停、恢复以及将音乐曲目跳转到上一首/下一首
- 获取、随机播放和清除当前播放列表
- 更改音乐和音效的音量

例如，向 AudioManager 类添加公共 `setMusicVolume()` 和 `getMusicVolume()` 方法来控制音乐音量：

```
void setMusicVolume(int volume);
int getMusicVolume();
```

此外，私有成员变量 `mMusicVolume` 用于存储当前音乐音量：

```
int mMusicVolume = 64;
```

对于函数列表中的其余功能，会创建公共方法和私有成员变量。查看 AudioManager.h 文件可看到 AudioManager 类中可用的全部方法和成员变量。

现在，我们必须向应用程序代码中添加一个 AudioManager 类对象的成员变量，以及音频功能的初始化和清理调用。为了将渲染器仅保留给视频部分，AudioManager 成员变量和方法将添加到 Window 类中。

### 将 AudioManager 类添加到 Window 类

将音频功能添加到 Window 类的两个主要步骤与其他所有类相同：在 Window.h 头文件顶部包含 AudioManager.h 头文件，并声明名为 `mAudioManager` 的私有成员变量：

```
#include "AudioManager.h"
...
AudioManager mAudioManager{};
```

然后，在 Window.cpp 文件中 Window 类的 `init()` 方法中，我们尝试初始化 AudioManager 对象：

```
if (!mAudioManager.init()) {
    Logger::log(1, "%s error: unable to init audio, skipping\n", __FUNCTION__);
}
```

注意，我们使用 Logger 类打印错误消息，但如果 AudioManager 初始化失败，我们不会终止窗口初始化。音频回放应保持可选，我们将继续运行而不进行音乐和音效回放，而不是使应用程序启动失败。

在初始化调用之后，可以立即将音乐曲目加载到 AudioManager 中：

```
if (mAudioManager.isInitialized()) {
    mAudioManager.loadMusicFromFolder("assets/music", "mp3");
}
```

在这里，如果初始化成功，我们将尝试将 `assets/music` 文件夹中的所有 MP3 文件加载到 AudioManager 中。同样，如果在资源文件夹中找不到音乐文件，我们也不会失败。

### 将音乐曲目添加到播放列表

在 AudioManager 类中，已经实现了从本地文件夹 `assets/music` 加载 MP3 和 OGG 文件到播放列表的支持。您可以将自己的音乐添加到 `assets/music` 文件夹中，因为应用程序启动时会填充播放列表。在实践环节中，有一个任务是为 UI 添加一个“刷新”按钮，允许您在应用程序运行时添加或移除音乐曲目。

最后，我们需要在关闭应用程序时清理 AudioManager。因此，我们在 Window 类的 `cleanup()` 方法中添加对 AudioManager 的 `cleanup()` 方法的调用：

```
void Window::cleanup() {
    mRenderer->cleanup();
    mAudioManager.cleanup();
    ...
}
```

由于我们在 Window 类中创建了 AudioManager，因此需要创建回调函数，以便从渲染器或用户界面访问音乐和音效功能。

### 使用回调使 AudioManager 类随处可用

通过使用这些回调函数，可以在代码中的任何位置更改音乐曲目或播放脚步声。作为示例，我们将在这里使用播放特定音乐曲目的功能，并以曲目名称作为参数。

首先，我们将名为 `playMusicTitleCallback` 的回调函数签名添加到 Callbacks.h 文件中：

```
using playMusicTitleCallback =
    std::function<void(std::string)>;
```

接下来，我们在 ModelInstanceCamData.h 文件的 ModelInstanceCamData 结构体中创建一个名为 `micPlayMusicTitleCallbackFunction` 的变量：

```
playMusicTitleCallback micPlayMusicTitleCallbackFunction;
```

我们使用 ModelInstanceCamData 结构体来避免回调函数散布在代码中。

然后，在 Window 类的 `init()` 方法中，将该回调变量绑定到 AudioManager 的相应函数：

```
ModelInstanceCamData& rendererMICData =
    mRenderer->getModInstCamData();
rendererMICData.micPlayMusicTitleCallbackFunction =
    [this](std::string title) {
        mAudioManager.playTitle(title);
    };
```

在这里，我们获取包含渲染器类的 ModelInstanceCamData 结构体的变量的引用，并通过 lambda 函数将 AudioManager 类的 `playTitle()` 方法赋值给它。

对于渲染器中应可用的每个音频方法，必须重复创建回调函数签名、向 ModelInstanceCamData 结构体添加变量、将回调函数绑定到 AudioManager 类方法这三个步骤。

现在，让我们看看音频管理器低层部分的实现。我们将使用 SDL，因为音频函数易于使用。

### 使用 SDL 作为底层

尽管 SDL 可以处理比音频更多的功能，但我们只使用 SDL 库提供的函数子集也是可行的。因此，我们不会使用 SDL 来处理窗口和键盘（这些由应用程序中的 GLFW 完成），而只使用 SDL 的声音回放和混音功能。

要使用 SDL 进行音频回放，我们需要核心 SDL 库以及名为 SDL_mixer 的独立混音器库。

#### 在 Windows 上安装 SDL 和 SDL_mixer

对于 Windows，GitHub 上提供了预编译版本的 SDL 和 SDL_mixer。请访问以下两个 URL 的 releases 页面，并下载适用于 Visual Studio 的最新稳定开发 zip 文件，这些文件名中包含 `devel` 和 `VC`。例如，当前适用于 Visual Studio 的 SDL 主库开发包名为 `SDL2-devel-2.30.9-VC.zip`。以下是 URL：

- SDL: https://github.com/libsdl-org/SDL/releases
- SDL_mixer: https://github.com/libsdl-org/SDL_mixer/releases

现在，将这两个 zip 文件解压到您的计算机上，但注意路径名中要避免空格和特殊字符（如德语元音变音），以免出现问题，因为即使在 2024 年，许多工具在路径中遇到特殊字符时仍然存在问题。

接下来，添加两个环境变量，以帮助 CMake 搜索脚本找到头文件和库：

- 第一个变量名为 `SDL2DIR`，必须指向 SDL 主库解压后的文件夹。
- 第二个变量名为 `SDL2MIXERDIR`，必须指向 SDL_mixer 库的文件夹。

图 14.1 展示了这些环境变量的示例：

![](img/3cfb9832c943f5f7366fd104b5255971_489_0.png)

图 14.1：SDL 的环境变量示例

