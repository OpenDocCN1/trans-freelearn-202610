

```
// The new way
// Build an AudioAttributes object
AudioAttributes audioAttributes = new AudioAttributes.Builder()
.setUsage(AudioAttributes.USAGE_ASSISTANCE_SONIFICATION)
.setContentType(AudioAttributes.CONTENT_TYPE_SONIFICATION)
.build();

// Initialize the SoundPool
sp = new SoundPool.Builder()
.setMaxStreams(5)
.setAudioAttributes(audioAttributes)
.build();
}
```

这里所示的 else 代码块当然会包含旧方法的代码。不需要 AudioAttributes 对象；我们只需通过传入同时播放的声音数量来初始化 SoundPool (sp)。最后一个参数用于声音质量，传入零即可。这比新方法简单得多，但在可选方面也缺乏灵活性：

```
else {
  // The old way
  sp = new SoundPool(5, AudioManager.STREAM_MUSIC, 0);
}
```

现在，我们已经在 assets 文件夹中准备好了声音效果，并以某种方式构建了 SoundPool 对象，可以继续加载（解压缩）SoundPool 类中的声音文件了。

## 将声音文件加载到内存中

与加载或保存磁盘文件的方法一样，我们需要将代码包装在 try-catch 块中。这很合理，因为读取文件可能会因超出我们控制的原因而失败，而且由于我们使用的方法会抛出异常，否则我们编写的代码无法编译，所以被迫这样做。

在 try 块中，我们声明并初始化 AssetManager 和 AssetFileDescriptor 类型的对象。这里使用 AssetManager 对象的 openFd 方法初始化 AssetFileDescriptor，该方法实际解压缩声音文件。然后，我们在将 AssetFileDescriptor 的内容加载到 SoundPool 的同时初始化我们的 ID (idFX1)。catch 块只是向控制台输出一条消息，以告知是否出了问题。请注意，无论 Android 版本如何，此代码都相同：

```
try{
    // Create objects of the 2 required classes
    AssetManager assetManager = this.getAssets();
    AssetFileDescriptor descriptor;

    // Load our fx in memory ready for use
    descriptor = assetManager.openFd("fx1.ogg");
    idFX1 = sp.load(descriptor, 0);
}catch(IOException e){
    // Print an error message to the console
    Log.e("error", "failed to load sound files");
}
```

现在，我们准备发出一些声音了。

## 播放声音

此时，我们的 SoundPool 类中有一个声音效果，并且有一个 ID 可以用来引用它。

无论我们如何构建 SoundPool 对象，此代码都相同，这就是播放声音的方法。请注意，我们使用实际播放声音的同一方法的返回值来初始化 nowPlaying 变量。因此，以下代码同时播放声音，并将正在播放的 ID 值加载到 nowPlaying 中。当我们制作一个真正的小应用来播放一些声音时，我们将看到这将有何用处：

```
nowPlaying = sp.play(idFX1, volume, volume, 0, repeats, 1);
```

play 方法的参数为：声音效果的 ID、左扬声器音量、右扬声器音量、相对于其他声音的优先级、声音重复次数，以及播放速率/速度（1 为正常速率）。

在制作真正的应用之前，还有一个快速的事情。

## 停止声音

使用 stop 方法停止正在播放的声音也非常简单。请注意，任何给定时间可能有多个声音效果同时播放，因此 stop 方法需要要停止的声音效果的 ID：

```
sp.stop(nowPlaying);
```

现在，我们将尝试一个尚未使用过的 UI 小部件的同时，真正地发出一些声音。

## 介绍 Spinner 小部件

当然，说了这么多音效，我们需要一些实际的声音文件。您可以从各种免费网站下载音效，也可以购买高级音效。此应用的音效包含在下载包中，位于 Chapter 17/Sound Demo 文件夹的 assets 文件夹里。但您也可以自己制作。

## 制作音效

有一个名为 Bfxr 的开源应用，允许我们制作自己的音效。以下是使用 Bfxr 快速制作自己音效的指南。从 www.bfxr.net 获取免费副本。

按照网站上的简单说明进行设置。尝试其中一些内容来制作酷炫的音效。

## 提示

这是一个非常精简的教程。使用 Bfxr 可以做很多事情。要了解更多信息，请阅读之前网址上的提示。

- 1. 运行 Bfxr，如图所示：

![](img/35d01831c19688a44e50bce7c489007a_567_0.png)

- 2. 尝试所有预设类型，如下一张屏幕截图所示，它们会生成该类型的随机声音。当您有了接近想要的声音时，进入下一步：

![](img/35d01831c19688a44e50bce7c489007a_568_0.png)

- 3. 使用滑块微调新声音的音调、时长和其他方面，如下图所示：

![](img/35d01831c19688a44e50bce7c489007a_569_0.png)

- 4. 单击 **导出 WAV** 按钮保存声音。尽管该按钮上的文字如此，但我们也可以以 .wav 以外的格式保存声音：

![](img/35d01831c19688a44e50bce7c489007a_569_1.png)

- 5. Android 与 OGG 格式的声音配合得很好，因此在要求命名文件时，请在文件名末尾使用 .ogg 扩展名。

- 6. 重复步骤 2 到 5 来创建三个酷炫的音效。将它们命名为 fx1.ogg、fx2.ogg 和 fx3.ogg。我们使用 .ogg 文件格式，因为它比 WAV 等格式压缩得更多。当您的声音文件准备好后，我们可以继续开发应用。

## 布局 Sound Demo 界面

与之前的项目相比，我会更简洁地描述我们正在熟悉的项目部分。不过，每次遇到新概念时，我都会确保进行完整解释。我想您将几个小部件拖放到 RelativeLayout 上并更改它们的文本属性应该没问题。

牢记这一点，请完成以下步骤。如果有任何问题，可以复制或查看下载包中 chapter 17/Sound Demo 文件夹里的代码：

- 1. 创建一个新项目，将其命名为 Sound Demo，并选择 Blank Activity。将所有其他设置保留默认值，并删除 Hello world! 小部件。

- 2. 按此顺序，从调色板中拖放 Spinner、SeekBar 和四个 Button 到布局上，同时排列它们并设置它们的文本属性，如下一张屏幕截图所示：

![](img/35d01831c19688a44e50bce7c489007a_572_0.png)

- 3. 使用以下表格设置我们在 Java 代码中需要的小部件属性：

| 小部件 | 要更改的属性 | 设置的值 |
|---|---|---|
| Spinner | id | spinner |
| Spinner | spinnerMode | dropdown |
| Spinner | entries | @array/spinner_options |
| SeekBar | id | seekBar |
| SeekBar | max | 10 |
| Button (FX 1) | id | btnFX1 |
| Button (FX 2) | id | btnFX2 |
| Button (FX 3) | id | btnFX3 |
| Button (STOP) | id | btnStop |

接下来，将以下高亮代码添加到 values 文件夹中的 strings.xml 文件中。我们在上一步中为 options 属性使用了名为 spinner_options 的字符串资源数组。它将表示我们 Spinner 小部件中可供选择的选项：

```
<resources>
<string name="app_name">Sound Demo</string>
<string name="hello_world">Hello world!</string>
<string name="action_settings">Settings</string>
<string-array name="spinner_options">
    <item>0</item>
    <item>1</item>
    <item>3</item>
    <item>5</item>
    <item>10</item>
</string-array>
</resources>
```

现在运行应用，起初您不会看到任何新内容。不过，如果您单击 Spinner 小部件，则会看到我们名为 spinner_options 的字符串数组中的选项。我们将使用 spinner 来控制声音效果播放时重复的次数，如下图所示：

