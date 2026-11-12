
## 项目 3

## 迪斯科舞池

在上一个项目中，我们教会了Processing如何“看”，更重要的是如何“跳舞”。我们的火柴人舞蹈团目前正在一个非常无聊的灰色舞池上跳舞。这很快就会改变，因为在这个项目中，我们将为我们的舞蹈团创建一个真正的迪斯科舞池。

照明的迪斯科舞池在70年代末由约翰·特拉沃尔塔主演的电影《周六夜狂热》而流行起来，我们将模拟一个由64块从下方灯光照亮的玻璃地砖组成的舞池。

### 任务简报

在这个项目中，我们将使用Processing配合Minim框架（由Damien Di Fede开发）来播放和分析音乐。我们还将使用它来实时访问音乐数据并实现一些音频可视化。Minim已随Processing捆绑提供，因此我们不需要为此任务安装库。

我们将创建多种音乐和定时器控制的可视化模式，并在音乐播放时让我们的草图在它们之间切换。然后我们将通过将正在创建的图形转换为3D对象的纹理，把它们变成迪斯科舞池。最后，我们将把纹理添加到我们在上一个项目中为火柴人舞蹈团创建的那个有些无聊的舞池上。

### 为什么它很棒？

说到音乐可视化，Processing是一个非常明智的选择。Minim库简化了对诸如节拍检测和快速傅里叶变换等功能的访问，而Processing提供了对最出色的3D和2D图形效果的访问。

默认的iTunes音乐可视化程序最初是一个名为"Magnetosphere"的Processing脚本，由艺术家Robert Hodgin（又名Flight404）编写。

## 你的挑战目标

我们的项目包含四个任务，即：

- 你能听见我吗？
- 随音乐闪烁
- 制作你的迪斯科舞池
- 舞者们来了

### 任务检查清单

此任务关乎为音乐创建出色的视觉效果，因此我们显然需要一些音乐曲目和一张声卡。这些先决条件不应该太难找到。如果你想完成此任务的最终任务，你还需要我们上一个任务的最终草图和一个Kinect，但前三个任务可以独立于我们的上一个任务进行。

撰写本文时，Processing 2.0 Beta不开箱即用支持Linux音频框架PulseAudio。如果你使用的是使用PulseAudio的Linux发行版（例如Ubuntu），你需要将Java PulseAudio库添加到Processing中。你可以在Processing问题跟踪系统中找到安装必要库的详细说明，网址为 http://code.google.com/p/processing/issues/detail?id=930。

## 你能听见我吗？

在这个任务中，我们将编写一个Processing草图来播放MP3文件，并使用Minim库访问播放的音频样本。我们将使用这些数据来编写一个类似示波器的曲线。然后我们将使用Minim的`fft`类来计算当前播放声音的频谱，并生成一个柱状图来可视化它。

### 启动推进器

让我们给Processing装上"耳朵"：

- 1. 创建一个新的Processing草图，并添加`setup()`和`draw()`方法。

  ```java
  void setup() {
  }

  void draw() {
  }
  ```

- 2. 通过导航到"草图 | 导入库 ... | `minim`"来添加Minim库的`import`语句。

- 3. 现在我们需要通过导航到"草图 | 添加文件 ..."或直接将其拖放到草图窗口来向我们的草图添加一个MP3文件。

- 4. 向你的草图添加一个AudioPlayer对象，并在setup()方法中初始化它。

  ```java
  import ddf.minim.spi.*;
  import ddf.minim.signals.*;
  import ddf.minim.*;
  import ddf.minim.analysis.*;
  import ddf.minim.ugens.*;
  import ddf.minim.effects.*;

  Minim minim;
  AudioPlayer player;

  void setup() {
    size(600, 300);
    minim = new Minim(this);
    player = minim.loadFile("loop.mp3", 1024);
  }

  void draw() {
  }
  ```

  如果你不使用示例代码提供的MP3文件，请务必将"loop.mp3"替换为你想要使用的MP3文件的文件名。

- 5. 要启动我们的播放器，我们添加一个mousePressed()方法，如果播放器当前没有在运行则调用loop()方法，否则调用pause()。

  ```java
  void mousePressed() {
    if (player.isPlaying()) {
      player.pause();
    } else {
      player.loop();
    }
  }
  ```

- 6. 现在我们通过将以下代码添加到我们的draw()方法中来创建一个示波器显示：

  ```java
  void draw() {
    background(0);
    stroke(0, 255, 0);

    for (int i = 0; i < player.bufferSize() - 1; i++) {
      float x1 = map(i, 0, player.bufferSize(), 0, width);
      float x2 = map(i + 1, 0, player.bufferSize(), 0, width);
      line(x1, 75 + player.mix.get(i) * 75, x2, 75 + player.mix.get(i + 1) * 75);
    }
  }
  ```

- 7. 到目前为止，我们已经访问了音频数据的样本数据来生成可视化。另一个非常有用的分析和可视化音频数据的工具是所谓的快速傅里叶变换——简称fft——它在给定时间计算我们声音的频谱。我们使用logAverages()方法来减少我们获得的频段数量。我们向草图添加一个新变量并在setup()方法中初始化它。

  ```java
  Minim minim;
  AudioPlayer player;
  FFT fft;

  void setup() {
    size(600, 300);
    minim = new Minim(this);

    player = minim.loadFile("loop.mp3", 1024);

    fft = new FFT(player.bufferSize(), player.sampleRate());
    fft.logAverages(11, 1);
  }
  ```

- 8. 在我们的draw()方法中，我们更新频谱并遍历fft对象为我们计算的频段，方法是添加以下代码：

  ```java
  void draw() {
    background(0);
    stroke(0, 255, 0);

    for (int i = 0; i < player.bufferSize() - 1; i++) {
      float x1 = map(i, 0, player.bufferSize(), 0, width);
      float x2 = map(i + 1, 0, player.bufferSize(), 0, width);
      line(x1, 75 + player.mix.get(i) * 75, x2, 75 + player.mix.get(i + 1) * 75);
    }

    noStroke();
    fill(255, 255, 0);
    fft.forward(player.mix);
    float step = width / 8;
    for (int i = 0; i < 8; i++) {
      float value = fft.getAvg(i) * 3;
      rect(step * i, height - value, step, value);
    }
  }
  ```

- 9. 现在运行草图并在草图窗口内单击以启动音乐。可视化效果应该如下一个截图所示：

  ![](img/5cb67baa7bc7778de360b14c00d1ab81_69_0.png)

## 任务完成 - 小结简报

在这个任务中，我们将Minim库导入了我们的草图并添加了一个MP3循环。我们使用声音播放器对象来访问和管理音乐的启动和停止，并使用混音器来访问样本并将其可视化为类似示波器的线条。

在步骤7中，我们添加了一个fft对象来访问当前播放样本的频谱。在这个草图中，我们使用频率数据来生成频谱的柱状图。使用fft对象可以访问的频段数量取决于我们AudioPlayer的缓冲区大小。如果你增加缓冲区，你会在频域中获得更高的精度，但你也需要更多的样本来计算缓冲区，因此你的可视化效果将不那么灵敏。如果你增加频段数量，你也需要更多的CPU功率来计算频谱。我们为下一个任务创建的可视化效果只需要八个频段，因此我们告诉Minim在多个频段上计算平均值。

## 机密情报

Minim是一个非常灵活和模块化的声音库。我们用来分析声音样本或频率的方法与声音播放器独立。如果你想可视化通过声卡线路输入麦克风录制的声音，你可以像这样用输入线替换AudioPlayer对象：

```java
AudioInput in;
in = minim.getLineIn();
```

## 随音乐闪烁

我们当前任务的第二个任务是创建可视化器，根据定时器或从Minim获得的音频信息生成一个8 x 8的地砖数组。地砖数组将由我们的draw()方法用于生成舞池的俯视图。

要在不同的可视化器之间切换，我们将编写一个在后台运行并每隔n秒选择一个新可视化器的线程。

### 启动推进器

让我们随音乐闪烁：

- 1. 让我们从一个新的草图开始并导入Minim库。在我们的`setup()`方法中，我们定义一个Minim上下文和一个`AudioPlayer`对象，就像在上一个任务中所做的那样。这次，我们直接在`setup()`方法中启动播放器的循环，因此草图一运行循环就会开始。

  ```java
  import ddf.minim.spi.*;
  import ddf.minim.signals.*;
  import ddf.minim.*;
  import ddf.minim.analysis.*;
  import ddf.minim.ugens.*;
  import ddf.minim.effects.*;

  Minim minim;
  AudioPlayer player;

  void setup() {
    size(300, 300);

    minim = new Minim(this);
    player = minim.loadFile("loop.mp3", 1024);
    player.loop();
  }
  ```
