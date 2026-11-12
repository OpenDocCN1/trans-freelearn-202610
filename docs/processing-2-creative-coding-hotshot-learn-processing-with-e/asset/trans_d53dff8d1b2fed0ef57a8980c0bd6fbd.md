

## 任务完成 - 小型简报

对于当前任务的第一个任务，我们创建了一个 Processing 草图来绘制我们的基本面部。我们添加了三个变量来控制用户可以更改的面部特征。我们将颜色模式更改为 HSB，以便用户更容易选择所需的颜色色调。与 RGB 类似，HSB 颜色模式也有三个参数，但不是混合红、绿、蓝，而是选择基本颜色（色相）、亮度和饱和度。为了简单起见，我们固定了亮度和饱和度值，并让用户更改色相值。

所有控制变量都存储 0 到 1023 之间的值，然后使用 map() 命令将其映射到绘制所需的值范围。

## 让我来改变它

没有人总是想微笑，但目前更改我们笑脸的面部参数有些麻烦，这可能会影响您当前的情绪状态。为了防止 Smillie-O-Mat 增加用户的挫败感或烦恼感，本次任务的第二项任务是在笑脸下方添加三个滑块，使用鼠标来调节面部参数。

## 启动推进器

现在让我们改变一下我们的脸：

- 1. 打开 Smillie-O-Mat 草图，并在 `setup()` 方法中更改草图窗口的大小，为笑脸下方的滑块腾出更多空间。

```java
void setup() {
  size(300, 390);
  smooth();
  background(255);
  colorMode(HSB);
}
```

- 2. 现在添加一个名为 `drawSliders()` 的新方法，并绘制三个黑色矩形作为滑块的基础。

```java
void drawSliders() {
  fill(0);
  strokeWeight(1);
  rect( 20,315,width-40,2 ) ;
  rect( 20,345,width-40,2 ) ;
  rect( 20,375,width-40,2 ) ;
}
```

- 3. 第一个滑块将改变嘴巴的位置，因此我们添加一个较小的矩形，并将嘴巴变量的值映射到矩形的 x 坐标。

```
void drawSliders() {
    fill(0);
    strokeWeight(1);
    rect( 20,315,width-40,2 );
    rect( 15 + map( mouth, 0, 1024, 0, width-40 ), 310, 10, 12 );
    rect( 20,345,width-40,2 );
    rect( 20,375,width-40,2 );
}
```

- 4. 添加一个用于眉毛角度的滑块和一个用于面部颜色的滑块，操作方式与处理嘴巴变量相同。

```
void drawSliders() {
    fill(0);
    strokeWeight(1);
    rect( 20,315,width-40,2 );
    rect( 15 + map( mouth, 0, 1024, 0, width-40 ), 310, 10, 12 );
    rect( 20,345,width-40,2 );
    rect( 15 + map( eye, 0, 1024, 0, width-40 ), 340, 10, 12 );
    rect( 20,375,width-40,2 );
    rect( 15 + map( col, 0, 1024, 0, width-40 ), 370, 10, 12 );
}
```

- 5. 现在我们从 draw() 方法中调用 drawSliders() 方法。

```
void draw() {
    background(255);
    drawSliders();
    color c = color(map(col, 0, 1024, 0, 255), 255, 255 );
    fill(c);
    ellipse(150,150,250,250);
    strokeWeight(3);
}
```

- 6. 运行您的草图，然后更改控制变量，检查滑块的位置是否发生变化。

- 7. 为了使滑块对鼠标做出反应，我们添加了一个名为 `mousePressed()` 的新方法和一个名为 `mclick` 的布尔变量。每次按下鼠标按钮时都会调用此方法；我们现在将测试鼠标单击是否发生在我们的嘴巴滑块上。

```
boolean mclick = false;
void mousePressed() {
 if ( mouseY > 310 && mouseY < 322 ) {
 mclick = true;
 mouth = constrain( int( map( mouseX, 20, width - 20, 0, 1024
 ) ), 0, 1024 );
 }
}
```

- 8. 我们需要添加另一个回调方法，当用户拖动鼠标时会调用该方法。只有当 `mclick` 变量由 `mousePressed()` 方法设置为 `true` 时，我们才更改滑块的位置。

```
void mouseDragged() {
 if ( mclick ) mouth = constrain( int( map( mouseX, 20, width - 20,
 0, 1024 ) ), 0, 1024 );
}
```

- 9. 当单击滑块时，我们将 `mclick` 变量设置为 `true`。当释放鼠标按钮时，我们需要将变量重新设置为 `false`。向我们的草图添加 `mouseReleased()` 方法。

```
void mouseReleased() {
 mclick = false;
}
```

- 10. 运行草图，拖动第一个滑块，并检查嘴巴的位置是否相应变化。

- 11. 为了使我们的眼睛和颜色滑块工作，我们需要两个更多的布尔变量，并且需要将以下代码添加到我们的 `mousePressed()` 方法中：

```
boolean mclick = false;
boolean eclick = false;
boolean cclick = false;

void mousePressed() {
 if ( mouseY > 310 && mouseY < 322 ) {
 mclick = true;
 mouth = constrain( int( map( mouseX, 20, width - 20, 0, 1024
 ) ), 0, 1024 );
 }

 if ( mouseY > 340 && mouseY < 352 ) {
 eclick = true;
 eye = constrain( int( map( mouseX, 20, width - 20, 0, 1024 ) ),
 0, 1024 );
 }

 if ( mouseY > 370 && mouseY < 382 ) {
 cclick = true;
 col = constrain( int( map( mouseX, 20, width - 20, 0, 1024 ) ),
 0, 1024 );
 }
}
```

- 12. 我们还需要通过向 mouseDragged() 和 mouseReleased() 方法添加以下代码来处理鼠标按钮的拖动和释放：

```
void mouseDragged() {
if ( mclick ) mouth = constrain( int( map( mouseX, 20, width - 20,
0, 1024 ) ), 0, 1024 );
if ( eclick ) eye = constrain( int( map( mouseX, 20, width - 20,
0, 1024 ) ), 0, 1024 );
if ( cclick ) col = constrain( int( map( mouseX, 20, width - 20,
0, 1024 ) ), 0, 1024 );
}
```

```
void mouseReleased() {
mclick = false;
eclick = false;
cclick = false;
}
```

- 13. 现在运行我们的草图，并使用滑块调整面部参数以匹配您当前的心情。在这张截图中，您可以看到可以生成的笑脸之一：

![](img/5cb67baa7bc7778de360b14c00d1ab81_105_0.png)

## 任务完成 - 小型简报

对于本次任务，我们添加了三个滑块来更改控制变量的值。我们没有使用任何现有的 GUI 库；我们从头开始创建了滑块，将落在 0 到 1024 区间内的变量值映射到滑块的长度，并绘制两个矩形。

我们还实现了三个回调函数来使滑块对鼠标事件做出反应。在 mousePressed() 方法中，我们测试鼠标指针是否位于我们的某个滑块上，如果是，则将该滑块的布尔变量设置为 true。在 mouseDragged() 方法中，如果该参数的布尔变量为 true，则更改控制变量的值。当释放鼠标按钮时，我们在 mouseReleased() 方法中将所有布尔变量设置为 false。

## Hello Twitter

我们任务中的第三项任务是创建一个能够更改我们当前 Twitter 状态的 Processing 草图。我们将使用 twitter4j.org 上的 Java 库 Twitter4J，它将所有 Twitter API 调用封装为方便的 Java 对象来设置状态。我们还将使用 Twitter 开发者网站来创建必要的应用程序密钥，并学习如何请求用户允许发布状态更新。

## 准备起飞

为了完成当前任务中的此项及后续任务，您需要一个用于发布推文的 Twitter 账户以及访问 Twitter 开发者网站的权限。如果您已经在使用 Twitter，那么您已满足此项任务的先决条件，因为 Twitter 不区分普通账户和开发者账户；每个 Twitter 用户都有权创建和注册应用程序。如果您还没有 Twitter 账户，请访问 twitter.com 并注册一个。

## 启动推进器

让我们发推文吧：

- 1. 访问 https://dev.twitter.com/ 上的 Twitter 开发者页面，并使用您的 Twitter 账户登录。

- 2. 现在单击您的账户，并从右上角的弹出式菜单中选择“我的应用程序”，如下图所示：

![](img/5cb67baa7bc7778de360b14c00d1ab81_107_0.png)

- 3. 现在单击 **创建新应用程序** 并填写注册表单来注册新应用程序。

- 4. 切换到 **设置** 标签页，并将 **应用程序类型** 更改为 **读写权限**，以允许我们的应用程序发布推文。

- 5. 现在切换回 **详情** 标签页，在 **消费者密钥** 中创建消费者密钥，并通过单击 **重新生成我的访问令牌** 创建 **消费者密钥** 密钥。下图显示了我的 Smilie-O-Mat 应用程序的 **详情** 页面：

![](img/5cb67baa7bc7778de360b14c00d1ab81_107_1.png)

