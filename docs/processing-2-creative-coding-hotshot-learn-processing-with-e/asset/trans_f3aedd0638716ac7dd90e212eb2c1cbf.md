

- 10. 在Kinect传感器前放置一个人，并让他/她移动。在下面的截图中，你可以看到作者移动他的咖啡杯：

![](img/5cb67baa7bc7778de360b14c00d1ab81_46_0.png)

## 目标完成 - 简要总结

我们刚刚下载并安装了OpenNI开源驱动程序，以访问Kinect传感器板。这些驱动程序允许我们从Kinect控制器访问红外和RGB图像。Kinect还使用红外摄像头进行深度扫描，并能够跟踪用户。

安装开发工具包、中间件驱动程序和Kinect控制器的传感器板后，我们使用开发工具包中的一个示例程序来测试安装。

## 机密情报

除了Kinect之外，还有其他使用PrimeSense技术并受OpenNI包支持的传感器条。要使用这些设备，你需要从 http://www.openni.org 安装传感器包，我们在启动推进器部分跳过了此步骤。

我使用Kinect运行并测试了本项目中的程序，但你也可以尝试使用其他传感器条，例如PrimeSense传感器或ASUS Xtion。

## 让Processing看见

我们的下一个任务是安装SimpleOpenNI库。该库使我们能够在Processing中使用OpenNI API。首先，我们将学习如何访问和显示Kinect控制器的深度图像。然后，我们将使用SimpleOpenNI框架的用户跟踪功能，并定义一些回调函数，以便在检测到用户或跟踪的用户消失时收到通知。最后，我们将为深度图像中属于用户的所有像素着色，以便用户能够看到正在跟踪的内容。

### 启动推进器

让我们教会Processing如何看见：

- 1. 在撰写本文时，SimpleOpenNI框架无法使用新的库管理器进行安装。要手动安装，请从 http://code.google.com/p/simple-openni/downloads/list 下载适用于您操作系统的SimpleOpenNI包。

- 2. 将其解压缩到`sketchbook`文件夹中名为`library`的文件夹。你可以在**首选项**对话框中找到该文件夹的路径。

- 3. 如果Processing当前正在运行，请重启它，以确保你的安装能找到该库。

- 4. 现在，创建一个新的草图，并在**草图**菜单下点击**导入库 ...** 以包含SimpleOpenNI库。

- 5. 向你的草图添加`setup()`和`draw()`方法。

```
import SimpleOpenNI.*;

void setup() {
}

void draw() {
}
```

- 6. 在`setup()`方法中，将草图窗口大小调整为640 x 480像素，并定义一个OpenNI上下文对象。我们使用`enableDepth()`方法开始捕获深度图像，并使用`setMirror()`激活镜像功能。

```
import SimpleOpenNI.*;
SimpleOpenNI context;
void setup() {
size( 640, 480 );
context = new SimpleOpenNI( this );
context.setMirror( true );
context.enableDepth();
}
```

- 7. 现在将以下行添加到你的 draw() 方法中，以便SimpleOpenNI加载下一帧并计算深度图像。然后，使用 context.depthImage() 以 PImage 对象的形式获取数据，并使用 image() 将其绘制到你的草图窗口中。

```
void draw() {
context.update();
image( context.depthImage(), 0, 0 );
}
```

- 8. 运行你的草图并对着摄像头挥手。你应该能看到你自己和周围环境的深度图像。

- 9. 在 setup 方法中通过添加以下行来启用用户检测：

```
void setup() {
size( 640,480 );
context = new SimpleOpenNI( this );
context.setMirror( true );
context.enableDepth();
context.enableUser( SimpleOpenNI.SKEL_PROFILE_ALL );
}
```

- 10. 你还需要一个变量来存储SimpleOpenNI框架检测到的第一个用户的 userId，因此在草图开头添加一个 player 变量并将其初始化为 -1。

```
int player = -1;
```

- 11. 为了在SimpleOpenNI检测到用户时收到通知，我们向草图添加以下两个回调方法：

```
void onNewUser( int userId ) {
println( "new user " + userId );
player = userId;
}
```

```
void onLostUser( int userId ) {
println( "lost user " + userId );
if ( userId == player ) {
player = -1;
}
}
```

- 12. 要查看Kinect在哪里发现了用户，你需要让图像中属于用户的所有像素都显示为绿色。你可以通过将以下代码添加到我们的 draw() 方法中来实现这一点：

```
void draw() {
 context.update();
 image( context.depthImage(), 0, 0 );

 loadPixels();
 if ( player != -1 ) {
 int[] userPixels = context.getUsersPixels( player );

 for( int p = 0; p < width * height; p++) {
 if ( userPixels[p] != 0 ) {
 pixels[p] = color( 0, green( pixels[p] ), 0 );
 }
 }
 }
 updatePixels();
}
```

- 13. 如果你现在运行代码，检测到的用户的像素应该变成绿色，如下面的截图所示：

![](img/5cb67baa7bc7778de360b14c00d1ab81_49_0.png)

## 目标完成 - 简要总结

我们刚刚学习了如何使用Processing访问Kinect提供的深度图像。从步骤 9 开始，我们使用了SimpleOpenNI框架的跟踪功能。我们添加了一个名为 onNewUser() 的回调函数，每次检测到用户时库都会调用它，还添加了一个名为 onLostUser() 的第二个函数，以便在用户不再在跟踪范围内时收到通知。

在我们的 draw() 方法中，我们使用了从 getUsersPixels() 方法获得的像素映射，如果深度图像中的像素属于用户则将其着色为绿色，否则保持不变。函数 getUsersPixels() 返回的数组中，每个非用户像素的值为 0，我们要着色的像素值为 1。对于属于用户像素的每个 1，我们获取深度图像的当前灰度值，并将其替换为相同强度的绿色值。

## 机密情报

除了红外和RGB摄像头外，Kinect还配备了电机控制，以允许调整摄像头倾斜度。不幸的是，OpenNI框架不支持控制电机。有一个名为 libfreenect 的第二个开源框架，可用于访问Kinect。它缺乏OpenNI的用户跟踪功能，但支持电机控制。因此，如果你需要调整Kinect的倾斜度，因为它臭名昭著地会切断玩家的头部或脚部，你可以安装 libfreenect 库和演示应用程序，并使用它们来调整你的Kinect。

## 制作舞者

在上一节中，我们使用OpenNI框架的用户跟踪功能来定位Kinect红外摄像头提供的深度图像中的用户。现在我们将进一步定位玩家的身体部位。我们将在本任务中使用的功能称为骨骼跟踪。OpenNI骨骼追踪器定位人体的某些关键点，我们将使用这些关键点来构建我们的火柴人。对于每个玩家，Kinect可以看到并获取头部、颈部、躯干、肩膀、手肘、手部、臀部、膝盖和脚部的位置。

我们还将上一节中使用的红外摄像头图像和用户像素作为一个小型抬头显示（HUD）显示，以便玩家能够看到Kinect正在跟踪的内容。

### 启动推进器

- 1. 我们需要创建一个新的Processing草图并导入SimpleOpenNI库。然后我们需要添加 setup() 和 draw() 方法。

```
import SimpleOpenNI.*;

void setup() {
}

void draw() {
}
```

- 2. 我们创建三个方法，由我们的 draw() 方法调用，用于绘制 HUD、舞者和舞池。

```
void drawHUD() {
}

void drawDancer() {
}

void drawFloor() {
}
```

- 3. 在我们的 draw() 方法中，我们需要添加对刚才创建的三个方法的调用。由于Kinect返回的坐标使用与Processing使用的不同的坐标系统（它默认会这样做），我们需要在调用 drawDancer() 和 drawFloor() 方法之前旋转并缩放我们的视角。

```
void draw() {
    background( 255 );
    context.update();

    drawHUD();

    translate( width/2, height/2, 0 );
    rotateX( PI );

    scale(0.5);
    translate(0, -100, -400);
    rotateY( radians( 30 ) );

    drawDancer();
    drawFloor();
}
```

