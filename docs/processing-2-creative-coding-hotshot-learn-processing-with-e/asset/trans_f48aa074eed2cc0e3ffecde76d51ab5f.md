
我们将舞者放在 draw() 方法中，并使用 translate() 方法在舞池的另一个位置绘制每个舞者。

## 启动推进器

让我们添加更多舞者：

- 1. 打开你在 *制作舞者* 任务中创建的草图，点击右侧的小箭头图标并从菜单中选择 **新建标签页** 来添加一个新类。新类名为 *人物*，因此请将标签页命名为 *人物* 并添加以下代码：
```
public class Figure {

}
```

- 2. 现在向你的类添加一个 `PVector` 对象数组（它将存储从 Kinect 获取的坐标）以及一个相同大小的数组，其中包含标识每个关节的 OpenNI 常量名称。
```
public class Figure {
PVector[] joints;
int[] ni_names = new int[] {
SimpleOpenNI.SKEL_HEAD,
SimpleOpenNI.SKEL_NECK,
SimpleOpenNI.SKEL_TORSO,
SimpleOpenNI.SKEL_LEFT_SHOULDER,
SimpleOpenNI.SKEL_LEFT_ELBOW,
SimpleOpenNI.SKEL_LEFT_HAND,
SimpleOpenNI.SKEL_RIGHT_SHOULDER,
SimpleOpenNI.SKEL_RIGHT_ELBOW,
SimpleOpenNI.SKEL_RIGHT_HAND,
SimpleOpenNI.SKEL_LEFT_HIP,
SimpleOpenNI.SKEL_LEFT_KNEE,
SimpleOpenNI.SKEL_LEFT_FOOT,
SimpleOpenNI.SKEL_RIGHT_HIP,
SimpleOpenNI.SKEL_RIGHT_KNEE,
SimpleOpenNI.SKEL_RIGHT_FOOT
};
}
```

- 3. 要初始化 `joints` 数组，你需要为你的 `Figure` 类添加一个构造函数，其形式如下：
```
public Figure() {
joints = new PVector[15];
for( int i=0; i<joints.length; i++ ) {
joints[i] = new PVector();
}
}
```

- 4. 你还需要向类中添加一些常量，以便在不必知道数组中数值索引的情况下，能够识别坐标数组中的条目。

```
public class Figure {
    static final int HEAD=0;
    static final int NECK=1;
    static final int TORSO=2;
    static final int L_SHOULDER=3;
    static final int L_ELBOW=4;
    static final int L_HAND=5;
    static final int R_SHOULDER=6;
    static final int R_ELBOW=7;
    static final int R_HAND=8;
    static final int L_HIP=9;
    static final int L_KNEE=10;
    static final int L_FOOT=11;
    static final int R_HIP=12;
    static final int R_KNEE=13;
    static final int R_FOOT=14;
}
```

- 5. 对于每一帧，Kinect 都会提供一组更新后的坐标；因此你需要添加一个 update() 方法，该方法获取 SimpleOpenNI 上下文和 userId 的引用。由于你已将坐标名称添加到数组中，可以简单地使用 for 循环将它们复制到你的 PVector 数组。

```
public void update( SimpleOpenNI context, int userId ) {
    for ( int i=0; i<joints.length; i++) {
    context.getJointPositionSkeleton( userId, ni_names[i], joints[i] );
    }
}
```

- 6. 你不希望每个人物都长得一模一样，因此需要对每个人物进行略微缩放。向类中添加以下变量：

```
float sx = 1;
float sy = 1;
float sz = 1;
color c = 0;
```

- 7. 现在添加一个名为 randomize() 的函数，它用于初始化缩放值并选择一种颜色。

```java
public void randomize() {
  sx = random( 0.9, 1.1 );
  sy = random( 0.9, 1.1 );
  sz = random( 0.9, 1.1 );

  colorMode( HSB );
  c = color( random( 255 ), 255, 255 );
  colorMode( RGB );
}
```

- 8. 你已初始化随机化值并更新了坐标；现在需要绘制你的火柴人。为了简化绘图，你需要添加一个名为 drawLimb() 的新方法，它获取你希望绘制线条的第一个和第二个坐标的索引。

```java
public void drawLimb(int idx1, int idx2) {
  line( joints[idx1].x, joints[idx1].y, joints[idx1].z,
        joints[idx2].x, joints[idx2].y, joints[idx2].z );
}
```

- 9. 要实现火柴人的实际绘制，你需要在 Figure 类中创建一个 draw() 方法。首先，你需要将绘图颜色设置为在 randomize() 方法中为人物生成的随机颜色，然后将缩放设置为你的 sx、sy 和 sz 值。

```java
public void draw() {
  strokeWeight( 2 );
  stroke( c );
  fill( c );
  pushMatrix();
  scale( sx, sy, sz );
}
```

- 10. 现在你需要添加代码来绘制头部和身体的线条。

```java
pushMatrix();
translate( joints[HEAD].x, joints[HEAD].y, joints[HEAD].z );
scale( .5, .5, 1);
sphere( joints[HEAD].dist( joints[NECK] ) );
popMatrix();
drawLimb( NECK, HEAD );

drawLimb( NECK, L_SHOULDER );
drawLimb( L_SHOULDER, L_ELBOW );
drawLimb( L_ELBOW, L_HAND );

drawLimb( NECK, R_SHOULDER );
drawLimb( R_SHOULDER, R_ELBOW );
drawLimb( R_ELBOW, R_HAND );

drawLimb( NECK, TORSO );
drawLimb( TORSO, L_HIP );
drawLimb( TORSO, R_HIP );
drawLimb( L_HIP, R_HIP );

drawLimb( L_HIP, L_KNEE );
drawLimb( L_KNEE, L_FOOT );

drawLimb( R_HIP, R_KNEE );
drawLimb( R_KNEE, R_FOOT );
popMatrix();
}
```

- 11. 切换回草图的主标签页，并将你在上一个任务中定义的 figure 变量更改为 Figure 对象的二维数组。

```
Figure figures[][] = new Figure[5][4];
```

- 12. 现在在 setup() 方法中添加以下代码行来初始化数组。为了让每个小舞者看起来略有不同，请调用你添加到 Figure 类中的 randomize() 方法：

```
void setup() {
size( 1024, 768, P3D);
context = new SimpleOpenNI( this );
context.enableDepth();

context.enableUser( SimpleOpenNI.SKEL_PROFILE_ALL );

smooth();
lights();

for ( int i = 0; i<5; i++ ) {
for ( int j=0; j<4; j++ ) {
figures[i][j] = new Figure();
figures[i][j].randomize();
}
}
}
```

- 13. 在 drawDancer() 方法中，你需要调用你创建的每个 figure 对象的 update() 函数。我们需要使用 translate() 函数将每个人物移动到不同的位置，以确保它们不会互相重叠绘制。

```
void drawDancer() {
if ( player != -1 && context.isTrackingSkeleton( player ) ) {
for ( int j=-2; j< 3; j++ ) {
for ( int k = 0; k < 4; k++ ) {
figures[j+2][k].update( context, player );
pushMatrix();
translate( 100*j, 0, -100*k );
scale( .1 );
figures[j+2][k].draw();
popMatrix();
}
}
}
}
```

- 14. 现在开始播放音乐，站在 Kinect 前面，开始跳舞吧。你的小火柴人将毫不疲倦地模仿你所有的精彩舞步。你可以在这个截图中看到我的舞蹈团：

![](img/5cb67baa7bc7778de360b14c00d1ab81_63_0.png)

## 任务完成 - 小型总结

在这个任务中，我们为火柴人定义了一个类。我们将访问 3D 坐标和绘制舞者的代码移动到我们的类中，以简化主草图中的 `drawDancer()` 方法。新类还使我们能够绘制多个舞者。

为了防止我们的舞蹈团看起来像克隆人大军，我们添加了 `randomize()` 方法，它会稍微缩放火柴人，并为每个人物选择一种随机颜色。

## 任务圆满完成

在这个项目中，我们通过连接 Kinect 并安装 OpenNI 框架，教会了 Processing 如何“看见”。

我们使用 SimpleOpenNI 框架提供的深度图像，并利用 OpenNI 的跟踪功能在图像中定位用户。对于每个被跟踪的用户，框架都会为我们提供一个位图，定义哪些像素属于被跟踪的用户，哪些不属于。在最后两个任务中，我们将第二个任务中创建的带有彩色像素的深度图像用作 HUD 显示，以便玩家可以看到当前正在跟踪的内容。

从第三个任务开始，我们使用骨骼跟踪器来访问玩家四肢和关节的 3D 坐标，并利用这些信息绘制出跟随玩家动作的火柴人。

在我们的最终任务中，我们将火柴人的绘图代码重构为一个类，并使用这个类来绘制多个舞者，这样我们的火柴人在跳舞时就有伙伴陪伴了。

## 准备好大干一场了吗？一项火热挑战

在这个项目中，我们只是初步探索了 Kinect 提供的可能性。在一个项目中不可能描述所有内容；你可以为此写一整本书。因此，尝试扩展你在本项目中获得的知识，尝试以下其中一项：

- 使用 RGB 摄像头的图像来显示玩家，而不是使用火柴人之一。
- 为你的舞者使用比火柴人更复杂的 3D 模型。
- 记录玩家的动作，并在短暂延迟后重放。
- 目前，我们的草图只支持一名玩家。OpenNI 框架也可以跟踪多个用户，因此尝试添加多人游戏支持。
