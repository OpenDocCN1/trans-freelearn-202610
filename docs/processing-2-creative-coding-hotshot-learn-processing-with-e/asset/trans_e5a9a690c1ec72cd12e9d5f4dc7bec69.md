

在第三个任务“更换你的面孔”中，我们扩展了在项目 4 中创建的 Smile-O-Mat 草图，以解析来自控制器的消息。我们实现了一个 serialEvent() 回调方法，每当消息到达串行端口时都会调用该方法。在此方法中，我们检查消息的第一个字母，如果它以 A、B 或 C 开头，则将字符串的其余部分解析为整数。这些整数用于控制我们笑脸的面部参数。如果消息是 click，则我们触发 tweet() 方法，该方法将当前笑脸发送到 tweeter。

当按钮被按下或释放时，存在一段短暂的时间，此时触点非常接近但尚未完全闭合。在这段时间内，按钮的短暂接触可能会意外触发多个点击事件。为了防止这种情况发生，我们创建了一个 50 毫秒的时间槽，在此期间不发送第二个点击消息。这种技术称为 debouncing，在处理开关或按钮时非常常见。

在最终任务“将其装入盒子”中，我们为控制器创建了外壳，并通过将元件焊接在一起使元件之间的连接永久化。我们没有使用电路板，而是直接将元件焊接在一起。我们使用铝制盒子作为外壳，并将旋钮排列成一条直线，但元件的布局和盒子的材质取决于个人喜好。这个控制器应该反映你的工作流程和个人偏好。按你喜欢的方式，让它尽可能传统或疯狂。

## 准备好大干一场了吗？一项热血挑战

Arduino 是一个极其酷炫的电子原型平台，适合初学者和专业人士。在这个项目中，我们当然只是触及了其可能性的表面。如果你喜欢钻研电子，尝试用以下任一方式扩展你的 Smile-O-Mat 草图：

- 添加更多旋钮以控制更多面部参数
- 添加电位器或旋转编码器以选择要推文的文本消息
- 添加 LCD 显示屏以显示最后一条推文
- 将一些舵机和 RGB LED 连接到 Arduino，创建面部的机械动画版本

## 项目 6

### 飞向月球

自从人们接触到计算机以来，他们就一直用它来玩游戏。最早的游戏之一是一款名为 Lunar Lander 的模拟游戏，玩家需要将月球登陆舱降落在月球上。第一个版本编写于 1969 年，是基于文本的。这款游戏的第一个图形版本是在四年后为 DEC 终端编写的。此后，月球登陆模拟游戏已被移植到几乎每一种计算机系统上，这使得它成为我们当前任务的完美游戏，因为我们将使用 Processing 模式，通过 Processing.js 让游戏在浏览器中运行，并在 Android 设备上运行。

### 任务简报

我们当前的任务是创建一款月球着陆器游戏。游戏的第一个版本将是一个在您的计算机上运行的 Processing 草图，类似于我们在之前任务中创建的草图。然后，我们将使用各种 Processing 模式，通过 JavaScript 模式让游戏在浏览器中运行，并通过 Android 模式在 Android 设备上运行。

对于这些模式中的每一个，如果有必要，我们将调整游戏控制以适应平台。

### 为什么这很棒？

这个项目很棒有两个原因：首先，我们正在编写游戏，自计算机发明以来，这一直是官方令人惊叹的计算机项目列表上的项目；其次，我们编写的游戏可以在您的计算机、Android 移动设备和浏览器中运行。这使我们能够涵盖当前几乎每个可上网的平台，从台式机和笔记本电脑到手机、平板电脑和智能电视。当 Java 问世时，它提出了“一次编写，到处运行”的承诺。Processing 正在将这一承诺提升到一个新水平。

### 你的高手目标

此任务分为四个子任务。我们将在前两个子任务中编写月球着陆器模拟游戏，并在剩下的两个子任务中让其在浏览器和 Android 设备上运行。以下是当前任务的目标列表：

- 绘制精灵图
- 启动着陆序列
- 在浏览器中运行你的草图
- 在 Android 手机上运行游戏

### 任务检查清单

此任务的前两个子任务仅使用 Processing 实现。在第三个子任务中，您需要一个支持 canvas 元素的浏览器，例如 Firefox、Chrome、Safari 或 Internet Explorer 8 及更高版本。

对于最后一个子任务，我们需要下载并安装 Android SDK，稍后将详细介绍。

### 绘制精灵图

当前任务的第一个子任务是创建一个显示飞船的精灵图，并使其在屏幕上移动和旋转。我们将使用 Processing 提供的 translate() 和 rotate() 方法。我们还将为游戏生成关卡设计，并为飞船生成一个着陆平台。

为了使游戏更有趣，这些关卡将在每次重新启动游戏时重新生成。

### 启动推进器

让我们开始创建关卡设计：

1. 创建一个新的 Processing 草图，并添加 setup() 和 draw() 方法，如下面的代码片段所示：

```
void setup() {
}

void draw() {
}
```

2. 现在我们将窗口大小设置为 300 乘以 300，并定义一个整数数组，我们将使用它来绘制月球。

```
int[] moon;

void setup() {
  size( 300, 300 );
  moon = new int[width/10+1];
  for ( int i=0; i < moon.length; i++ ) {
    moon[i] = int( random( 10 ) );
  }
}
```

3. 在我们的 draw() 方法中，我们添加一个浅蓝色网格，填满白色页面，以使其看起来像我们在数学笔记本的一页上制作了关卡。

```
void draw() {
  background(255);
  stroke(200, 200, 255);
  for ( int i=0; i<height/10; i++ ) {
    line( 0, i*10, width, i*10 );
  }

  for ( int i=0; i<width/10; i++ ) {
    line( i*10, 0, i*10, height );
  }
}
```

4. 要绘制月球，我们遍历在 setup() 方法中初始化的数组，并用透明黄色填充它。我们将向 draw() 方法添加一个 drawMoon() 方法，并使用 beginShape() 和 endShape() 创建我们的黄色多边形。

```
void draw() {
  background(255);
  stroke(200, 200, 255);
  for ( int i=0; i<height/10; i++ ) {
    line( 0, i*10, width, i*10 );
  }

  for ( int i=0; i<width/10; i++ ) {
    line( i*10, 0, i*10, height );
  }

drawMoon();

}

void drawMoon() {
stroke(0);
fill(255, 200, 0, 60);
beginShape();
vertex(0, height);
for ( int i=0; i < moon.length; i++) {
vertex( i * 10, height - 20 - moon[i] );
}
vertex(width, height);
endShape(CLOSE);
}
```

5. 运行你的草图；它应该看起来像下面的截图：

![](img/5cb67baa7bc7778de360b14c00d1ab81_152_0.png)

6. 在这个游戏中，玩家应该将火箭降落在着陆平台上。我们将把平台与步骤 3 中创建的网格对齐，以使其看起来像是我们给网格中的一些方框上了颜色。添加一个变量来存储着陆平台的 x 坐标，并在 setup() 方法中对其进行初始化。

```
int[] moon;
int landingX = 0;

void setup() {
size( 300, 300 );
moon = new int[width/10+1];
for ( int i=0; i < moon.length; i++) {
moon[i] = int( random( 10 ));
}
landingX = int( random(3, moon.length-4) )*10;
}
```

7. 现在，我们将一个 `drawLandingZone()` 方法添加到我们的 `draw()` 方法中，并通过绘制一个矩形和两条表示起落架的线来实现它。我们将使用 moon 数组来计算我们的起落架将接触地面的位置。

```
void draw() {
background(255);
stroke(200, 200, 255);
for ( int i=0; i<height/10; i++) {
line( 0, i*10, width, i*10);
}
for ( int i=0; i<width/10; i++) {
line( i*10, 0, i*10, height );
}
drawMoon();
drawLandingZone();
}
```

```
void drawLandingZone() {
fill(128, 200);
rect( landingX - 30, height - 50, 60, 10);
line( landingX - 30, height - 20 - moon[landingX/10-3], landingX
- 20, height - 40 );
line( landingX + 30, height - 20 - moon[landingX/10 +3],
landingX + 20, height - 40 );
}
```

8. 我们有了月球和着陆区，但还没有一艘可以降落的飞船。因此，绘制一张火箭图片，或从书籍的支持材料中下载图片，并通过将其拖放到草图窗口或使用“草图 | 添加文件...”菜单将其添加到你的草图中。

9. 现在我们将添加一个 PImage 变量，并在 setup() 方法中加载图片。

```
int[] moon;
int landingX = 0;
PImage ship;
void setup() {
size( 300, 300 );
moon = new int[width/10+1];
for ( int i=0; i < moon.length; i++) {
moon[i] = int(random(10));
}
landingX = int( random(3, moon.length-4) )*10;
ship = loadImage( "ship.png" );
}
```

