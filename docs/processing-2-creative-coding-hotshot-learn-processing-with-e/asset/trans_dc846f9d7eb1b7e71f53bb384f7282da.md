
String[] rawLines = loadStrings("romeo_and_juliet.txt");
ArrayList lines = new ArrayList();
for (int i = 0; i < rawLines.length; i++) {
  if (!"".equals(rawLines[i])) {
    String[] tmp = rawLines[i].split("#");
    lines.add(new Line(tmp[0], tmp[1].trim()));
  }
}

TTS romeo = new TTS();
TTS juliet = new TTS();
juliet.setPitchShift(2.4);
drama = new Drama(lines, romeo, juliet);

- 2. 切换到 Drama 线程，并为我们的角色添加两个变量。

```
public class Drama extends Thread {
TTS romeo;
TTS juliet;

int current;
ArrayList lines;
boolean running;
...
}
```

- 3. 我们还需要扩展 Drama 类的构造函数，以便添加角色。

```
public Drama(ArrayList lines, TTS romeo, TTS juliet) {
this.lines = lines;
this.romeo = romeo;
this.juliet = juliet;
current = 0;
running = false;
}
```

- 4. 在 run() 方法中，我们获取当前行，并根据添加到每行中的 actor 变量选择角色对象。我们也不再需要 delay() 和 println() 方法。

```
public void run() {
running = true;

for (int i = 0; i < lines.size(); i++) {
  current = i;
  Line l = (Line) lines.get(i);

  if ("J".equals(l.speaker)) {
    juliet.speak(l.text);
  }
  if ("R".equals(l.speaker)) {
    romeo.speak(l.text);
  }
}
running = false;
}
```

- 5. 由于我们的每个机器人都有自己的扬声器，我们不想在两个扬声器上都听到文本。我们希望一个机器人使用右声道，另一个机器人使用左声道。幸运的是，ttslib 提供了 speakLeft() 和 speakRight() 方法，正好满足我们的需求。因此，将两个 speak() 方法更改为如下所示：

```
...
if ("J".equals(l.speaker)) {
  juliet.speakLeft(l.text);
}
if ("R".equals(l.speaker)) {
  romeo.speakRight(l.text);
}
...
```

- 6. 目前，我们的 draw() 方法有些乏味，而且如果 drama 线程已经在运行，也有些误导。因此，我们将更改它以显示当前正在读取的脚本的五行。向 draw() 方法添加一个 if 语句，以检查我们之前添加到 Drama 线程的 running 变量的状态。

```
void draw() {
  background(255);
  textAlign(CENTER);
  fill(0);

  if (!drama.isRunning()) {
    text("Click here for Drama", width/2, height/2);
  } else {
  }
}
```

- 7. 现在我们添加一个 `for` 循环，在草图窗口中显示前两行、当前正在读取的行以及接下来的两行文本。我们现在将更改文本对齐方式，以便与机器人角色的扬声器匹配。如果我们的机器人使用 `speakRight()`，文本将右对齐；如果使用 `speakLeft()`，文本将左对齐。

```
void draw() {
  background(255);
  textAlign(CENTER);
  fill(0);

  if (!drama.isRunning()) {
    text("Click here for Drama", width/2, height/2);
  } else {
    int current = drama.getCurrent();
    for (int i = -2; i < 3; i++) {
      Line l = drama.getLine(i + current);
      if (l != null) {
        if ("J".equals(l.speaker)) {
          textAlign(LEFT);
          text(l.text, 10, height/2 + i * 30);
        } else {
          textAlign(RIGHT);
          text(l.text, width - 10, height/2 + i * 30);
        }
      }
    }
  }
}
```

- 8. 为了更突出地显示当前行，我们将更改文本颜色。我们将当前行的颜色设置为黑色，所有其他行设置为较浅的灰色，方法是在 for 循环中添加一个 `fill()` 语句。

```
for (int i = -2; i < 3; i++) {
  fill(abs(i) * 100);
  Line l = drama.getLine(i + current);
  if (l != null) {
    if ("J".equals(l.speaker)) {
      textAlign(LEFT);
      text(l.text, 10, height/2 + i * 30);
    } else {
      textAlign(RIGHT);
      text(l.text, width - 10, height/2 + i * 30);
    }
  }
}
```

- 9. 现在运行你的代码，单击草图窗口以启动 drama 线程。

## 任务完成 - 简要汇报

在我们当前任务的这一步骤中，我们添加了两个 TTS 对象并更改了声音参数，使它们听起来不同（在步骤 1 中）。然后我们扩展了 Drama 线程，并为机器人角色的声音添加了 TTS 对象。在步骤 4 和 5 中，我们修改了 run 方法，以使用我们刚刚创建的声音，而不仅仅是打印文本行。

在步骤 6 到 9 中，我们对 draw() 方法进行了更改，使其显示五行文本。当前正在播放的行是黑色的，前后两行则淡化为浅灰色。

fill() 方法不仅用于更改对象的填充颜色，还用于更改文本颜色。因为我们的 for 循环索引从 -2 到 2，我们可以简单地取其绝对值并乘以 100 来获取灰度级别。以下是运行中的草图的截图：

![](img/5cb67baa7bc7778de360b14c00d1ab81_33_0.png)

## 机密情报

FreeTTS 和 ttslib 还允许你使用名为 MBROLA 的二进制 TTS 引擎。不幸的是，它只以二进制形式分发，并且在撰写本文时，它只适用于 Linux。所以，如果你正在使用 Linux 并想尝试一下，可以对我们的 Romeo and Juliet 草图做出以下更改：

- 1. 在浏览器中打开 http://ctts.fpms.ac.be/synthesis/mbrola.html 并点击 Download。下载适用于你平台的 MBROLA 二进制文件。
- 2. 从 MBROLA 网站下载 female_us1 音色。
- 3. 为 MBROLA 二进制文件创建一个文件夹，并解压你刚刚下载的两个包。确保 MBROLA 二进制文件的路径中不含空格，因为 FreeTTS 无法处理空格。
- 4. 将 MBROLA 二进制文件重命名为 mbrola。
- 5. 现在回到你的 Romeo and Juliet 草图，并在 setup() 方法中添加以下突出显示的行：

```
void setup() {
  System.setProperty("mbrola.base", "/path/to/mbrola/");

  size(800, 400);
  textFont(createFont("Georgia", 24));

  String[] rawLines = loadStrings("romeo_and_juliet.txt");
  ArrayList lines = new ArrayList();
  for (int i = 0; i < rawLines.length; i++) {
    if (!"".equals(rawLines[i])) {
      String[] tmp = rawLines[i].split("#");
      lines.add(new Line(tmp[0], tmp[1].trim()));
    }
  }

  drama = new Drama(lines);
  TTS romeo = new TTS();
  TTS juliet = new TTS();
  juliet.setPitchShift(2.4);
  drama = new Drama(lines, romeo, juliet);
}
```

- 6. 将系统属性的路径指向包含你的 mbrola 二进制文件和 us1 音色的文件夹。
- 7. 现在你可以将 Juliet TTS 对象更改为以下内容：`TTS juliet = new TTS("mbrola_us1");`
- 8. 你还需要更改声音的音调，因为 mbrola_us1 已经是女性声音，我们不再需要模拟它了。

MBROLA 是在比利时蒙斯理工学院开发的文本转语音引擎。作者要求每篇提及其作品的出版物都必须提及他们的书籍，《文本转语音合成导论》，Thierry Dutoit，Kluwer Academic Publishers，多德雷希特，精装，ISBN 1-4020-0369-2，1997 年 4 月，312 页。

## 构建机器人

现在我们准备好进行 Romeo and Juliet 项目的最后一项任务了。我们将拿一些纸箱、泡沫塑料球和一对廉价扬声器或耳机，将它们变成我们的机器人演员。

本节中描述的机器人只是我的版本。如果你喜欢，可以按照自己的想法构建完全不同的机器人，可以复杂也可以简单。

## 为起飞做准备

要构建我们的机器人，我们需要一些材料和工具，这些材料和工具不应该太难找到。我为我的机器人演员使用了以下材料：

- 两个用于机身的纸箱
- 两个用于头部的泡沫塑料球
- 一对廉价扬声器或耳机
- 活动眼珠
- 丙烯酸颜料
- 记号笔
- 针
- 热熔胶枪

以下图片显示了我用于机器人的主要材料：

![](img/5cb67baa7bc7778de360b14c00d1ab81_36_0.png)

## 启动推进器

让我们来构建一些机器人：

- 1. 拆解你的扬声器，尝试去掉外壳。我们只需要扬声器，但请确保不要移除或破坏电缆。
- 2. 在纸箱上剪一个扬声器应安装的位置的孔。让孔稍微比扬声器小一点，如下图所示，因为我们稍后需要将它们粘到盒子上：

![](img/5cb67baa7bc7778de360b14c00d1ab81_36_1.png)
