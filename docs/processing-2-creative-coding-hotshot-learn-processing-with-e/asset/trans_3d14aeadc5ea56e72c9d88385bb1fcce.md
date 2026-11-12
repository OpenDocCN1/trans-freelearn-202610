

桌面3D打印仍远未像《星际迷航》中的复制器那样易于使用或功能全面，但我认为这是朝着正确方向迈出的非常有希望的一步。

## 你的紧迫目标

要完成当前任务，我们需要承担以下任务：

- 优美的函数
- 生成对象
- 导出对象
- 让它成为现实

## 任务清单

我们的当前任务是创建并导出一种可在3D打印机上打印的格式的3D对象。所有示例在无需任何额外要求的情况下即可运行，但如果你想跟进最后一项任务，则需要能够使用3D打印机或在线3D打印服务。

## 优美的函数

当前任务的第一项是创建一个可作为花瓶横截面的二维形状。我们将使用极坐标创建圆形，并使用一个根据旋转角度改变半径的函数，使其看起来更有趣。

我们还将使用Andreas Schlegel的controlP5库添加一个GUI，使用户能够修改我们草图中的曲线。

## 点燃推进器

让我们开始绘制曲线：

- 1. 启动一个新的Processing草图并添加`setup()`和`draw()`方法。
  ```
  void setup() {
  }
  void draw() {
  }
  ```

- 2. 现在添加两个float数组，用于保存我们形状的顶点坐标。同时在`setup()`方法中添加初始化代码。我们将每5度使用一个顶点，因此我们的形状需要72个顶点。
  ```
  float vertx[];
  float verty[];
  ```

```
void setup() {
    size(400,400);
    vertx = new float[72];
    verty = new float[72];
}
```

3. 要创建坐标值，我们添加两个方法；一个用于计算给定角度的极坐标半径，另一个使用此半径计算顶点的笛卡尔坐标并将其存储在我们的数组中。

```
float getR( float a ) {
    return 100;
}
```

```
void initPoints() {
    for( int a = 0; a < 72; a++) {
        vertx[a] = cos( radians( a * 5.0 ) ) * getR( a * 5.0 );
        verty[a] = sin( radians( a * 5.0 ) ) * getR( a * 5.0 );
    }
}
```

4. 在我们的`draw()`方法中，我们需要添加对`initPoints()`方法的调用。然后我们可以遍历数组并创建一个形状。

```
void draw() {
    pushMatrix();
    background(255);
    translate( width/2, height/2 );
    noFill();
    initPoints();
    beginShape();
    for( int a = 0; a < 72; a++) {
        vertex( vertx[a], verty[a] );
    }
    vertex( vertx[0], verty[0] );
    endShape();
    popMatrix();
}
```

5. 目前，我们的`getR()`函数对每个角度返回相同的半径，如果我们想绘制圆形，这没问题。但因为我们想创建更有趣的形状，所以我们将以下代码添加到我们的`getR()`函数中：

```
float getR( float a ) {
    return sin( radians(a)*6 ) * 20 + 100;
}
```

6. 运行你的草图。我们创建的形状应该与这个截图中的一样：

![](img/5cb67baa7bc7778de360b14c00d1ab81_232_0.png)

7. 现在我们需要一种简单的方法让用户更改我们刚刚创建的函数的参数。我们将使用controlP5库创建GUI，所以让我们使用库管理器安装它。转到Sketch | Import Library... | Add Library...打开库管理器，然后在搜索字段中输入controlP5。

8. 点击这个截图中对话框中的Install按钮：

![](img/5cb67baa7bc7778de360b14c00d1ab81_232_1.png)

9. 现在让我们在Sketch | Import Library...中选择ControlP5，将库导入到我们的草图。

10. 我们将添加一个组框，作为我们的控制面板，以及一个按钮，允许用户切换菜单的可见性。

```
import controlP5.*;

ControlGroup box;
ControlP5 cp5;
Button toggleButton;

float vertx[];
float verty[];
```

11. 为了将GUI设置代码与形状初始化分开，我们添加一个名为setupGUI()的方法，并从setup()方法中调用它。

```
void setup() {
size(400,400);
setupGUI();
vertx = new float[72];
verty = new float[72];
}

void setupGUI() {
}
```

12. 在我们的setupGUI()方法中，我们创建切换按钮和菜单框。

```
void setupGUI() {
cp5 = new ControlP5(this);
toggleButton = cp5.addButton("toggleBox", 1, 0, 0, 100, 20);
toggleButton.setLabel("Hide Menu");

box = cp5.addGroup( "box", 0, 0, 150);
box.setBackgroundHeight(120);
box.setBackgroundColor(color(0, 100));
}
```

13. ControlP5为每个GUI元素使用回调函数。函数名称是我们在setupGUI()函数中使用的addButton()方法的第一个参数。因此，当有人点击我们的按钮时会收到通知的回调方法必须命名为toggleBox()。

```
void toggleBox(int theValue) {
if (box.isVisible()) {
toggleButton.setLabel("Show Menu");
box.hide();
} else {
toggleButton.setLabel("Hide Menu");
box.show();
}
}
```

14. 目前，我们的菜单框完全是空的。让我们添加两个滑块来改变这一情况，一个用于控制曲线的振荡次数，另一个用于控制振荡大小。我们将振荡次数限制为整数，因为我们不希望函数中出现任何难看的跳跃。

```
void setupGUI() {
    cp5 = new ControlP5(this);
    toggleButton = cp5.addButton("toggleBox", 1, 0, 0, 100, 20);
    toggleButton.setLabel("Hide Menu");

    box = cp5.addGroup( "box", 0, 0, 150);
    box.setBackgroundHeight(120);
    box.setBackgroundColor(color(0, 100));

    Slider slider = cp5.addSlider("slider");
    slider.setPosition(10, 50);
    slider.setSize(80, 10);
    slider.setRange(1, 8);
    slider.setValue(6);
    slider.setNumberOfTickMarks(8);
    slider.setLabel( "count" );
    slider.moveTo( box );

    Slider slider2 = cp5.addSlider("slider2");
    slider2.setPosition(10, 80);
    slider2.setSize(80, 10);
    slider2.setRange(-20, 20);
    slider2.setValue(10);
    slider2.setLabel( "radius" );
    slider2.moveTo( box );
}
```

15. 现在我们需要添加两个float变量来存储我们的参数，以及两个在用户移动滑块时被调用的回调函数。

```
float v1 = 0;
float v2 = 0;

void slider( float val ) { v1 = val; }
void slider2( float val ) { v2 = val; }
```

16. 然后我们将使用这两个float变量来影响`getR()`方法中的半径。

```
float getR( float a ) {
   return sin( radians(a)*v1 ) * v2 + 100;
}
```

17. 现在运行你的草图并更改滑块值。在下面的截图中，你可以看到编辑形状后GUI的外观：

![](img/5cb67baa7bc7778de360b14c00d1ab81_235_0.png)

## 目标完成 - 简要汇报

在当前任务的第一项中，我们创建了一个可作为花瓶横截面的二维闭合形状。我们使用极坐标，通过根据角度改变半径来创建形状的顶点。

为了让用户能够在不更改源代码的情况下更改曲线的参数，我们添加了一个GUI。从第6步开始，我们安装了controlP5库并添加了一个按钮和一个菜单框。该按钮通过在回调函数中调用`show()`和`hide()`方法来切换菜单框的可见性。

从第14步开始，我们向菜单框添加了两个滑块，允许我们控制曲线上振荡的数量和大小。

## 生成对象

当前任务的第二项是生成一个可用作小笔筒或花瓶的3D对象。我们将使用任务1中创建的二维形状作为对象的横截面，并沿z轴将其拉伸。我们将扩展getR()方法并添加一堆额外的控制变量，允许我们根据对象的高度改变圆形形状。

为了使用户更容易查看对象，我们将添加mousePressed()和mouseDragged()函数，这些函数可以将对象朝任何方向旋转。我们将使用名为quaternion的数学结构来实现这些旋转。

## 点燃推进器

让我们将形状带入第三维：

- 1. 要将我们的形状转换为3D对象，我们需要扩展vertx和verty数组。

```
import controlP5.*;
ControlGroup box;
ControlP5 cp5;
Button toggleButton;
float[][] vertx;
float[][] verty;
```

- 2. 在我们的setup()方法中，我们将显示模式更改为P3D并定义数组初始化。我们的形状将由20个三角形条组成，每个包含72个顶点。

```
void setup() {
    size(400, 400, P3D);
    setupGUI();
    vertx = new float[20][72];
    verty = new float[20][72];
}
```

- 3. 因为我们更改了顶点数组，所以也需要更改initPoints()方法。我们将添加一个第二个for循环来遍历高度。

```
void initPoints() {
    for ( int h = 0; h < 20; h++) {
        for ( int a = 0; a<72; a++) {
            float r = getR( a*5.0 );
            vertx[h][a] = cos( radians( a*5.0 ) ) * r;
```
