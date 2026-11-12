

- 10. 在我们的 draw() 方法中，添加对名为 drawShip() 的方法的调用，并实现此方法。

```java
void draw() {
  background(255);
  stroke(200, 200, 255);
  for ( int i=0; i<height/10; i++) {
    line( 0, i*10, width, i*10 );
  }
  for ( int i=0; i<width/10; i++) {
    line( i*10, 0, i*10, height );
  }

  drawMoon();
  drawLandingZone();
  drawShip();
}
```

```java
void drawShip() {
  image(ship, 150, 100);
}
```

- 11. 目前，我们可以通过更改 x 和 y 坐标将飞船放置在想要的位置，但我们还需要能够旋转飞船，因为在我们的游戏中，玩家必须转动飞船并点燃推进器，以便将其引导至平台并尽可能平稳地着陆。为了解决这个问题，我们将使用 rotate() 和 translate() 命令。我们还将使用 pushMatrix() 和 popMatrix()，以确保我们的 rotate() 方法仅影响图像，而不影响整个窗口。

```java
void drawShip() {
  pushMatrix();
  translate(150, 100);
  rotate(PI/6);
  image( ship, -ship.width/2, -ship.height/2, ship.width, ship.height );
  popMatrix();
}
```

- 12. 如果您现在运行草图，火箭将以 30 度的角度悬停在月球上方，但月球、着陆平台和我们的网格保持不变。

![](img/5cb67baa7bc7778de360b14c00d1ab81_155_0.png)

## 任务完成 - 简要总结

在当前任务的第一个任务中，我们创建了游戏所需的所有图形元素。在步骤 1 到 3 中，我们创建了一个看起来像数学笔记本的网格，使游戏看起来像是在课堂上画出来的（当然是课间休息时！）。

在步骤 4 中，我们添加了随机生成的月球地貌。我们在 `setup()` 方法中随机生成了月球地面高度的偏移量，并将其存储在一个数组中。这样，我们可以确保月球在一局游戏中保持不变，但在玩家完成关卡并重新启动时可以轻松重新生成。要绘制月球，我们使用 `beginShape()` 和 `endShape()` 方法创建了一个复杂的多边形，并使用 `vertex()` 命令定义了多边形的顶点。

着陆平台由一个与网格对齐的矩形组成，使其看起来像是我们给其中一些方格上了色。我们还为平台添加了支架，并使用前一节中的数组使它们接触月球的地面高度。

最后，我们使用了飞船图像并创建了一个变换上下文，以便能够旋转它。我们需要这个新上下文，以确保我们对 `translate()` 和 `rotate()` 的调用仅影响用于飞船的绘制操作，而不影响屏幕上的所有其他部分。新上下文通过 `pushMatrix()` 开始，通过 `popMatrix()` 结束。我们在此上下文中使用的每个缩放、平移或旋转操作都只影响其自身的绘制操作。我们的网格、月球和着陆平台不受其影响。

## 启动着陆程序

当前任务的第二个任务是实现我们月球登陆者游戏的物理模拟和游戏控制。在此任务中，游戏应能够在运行 Java 的台式计算机和笔记本电脑上运行。我们将添加键盘控制，使玩家能够将火箭向左和向右转动，并且当玩家按下向上光标键时，推进器将点火并将火箭在其转向的方向加速一点。

## 点火推进器

让我们先通过添加月球重力来开始实现物理效果：

- 1. 我们需要为月球物理模拟实现的第一件事是用于存储飞船位置、方向、当前速度和月球重力强度的变量。

```java
int[] moon;
int landingX = 0;
PImage ship;

PVector pos = new PVector( 150, 20 );
PVector speed = new PVector( 0, 0 );
PVector g = new PVector( 0, 1.622 );
```

- 2. 现在我们需要在每一帧更新飞船的位置，因此我们在草图中添加一个 update() 方法，并从 draw() 方法中调用它。

```java
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
    drawLandingZone();
    drawShip();

    update();
}

void update() {
}
```

- 3. 我们还需要更改 `drawShip()` 方法中对 `translate()` 命令的调用，以使用新的 `pos` 变量。

```java
void drawShip() {
  pushMatrix();
  translate(pos.x, pos.y);
  rotate(PI/6);
  image( ship, -ship.width/2, -ship.height/2, ship.width, ship.height );

  popMatrix();
}
```

- 4. 在这个 `update()` 方法中，我们将实现所有物理计算。那么，让我们先实现重力对飞船的影响。

```java
void update() {
  PVector gDelta = new PVector( g.x / frameRate, g.y / frameRate);
  speed.add( gDelta );
  pos.add( speed );
}
```

- 5. 由于月球表面非常坚固，我们还添加了一个测试，一旦飞船到达表面就停止它。

```java
void update() {
  PVector gDelta = new PVector( g.x / frameRate, g.y / frameRate);

  speed.add( gDelta );
  pos.add( speed );

  if ( pos.x > landingX - 40 && pos.x < landingX + 40 && pos.y >
  height-50 - ship.height/2 ) {
    pos.y = height - 50 - ship.height/2;
  } else if (pos.y > height - 20 - ship.height/2 ) {
    pos.y = height - 20 - ship.height/2;
  }
}
```

- 6. 现在运行您的草图。火箭应该从窗口顶部开始，掉到底部并停在那里，如下面的屏幕截图所示：

![](img/5cb67baa7bc7778de360b14c00d1ab81_158_0.png)

- 7. 由于游戏的目标不是将火箭撞向月球，而是将其着陆，因此我们需要实现一个减速火箭的方法。游戏将使用键盘控制，因此让我们添加加速度和旋转角度变量。

```java
int[] moon;
int landingX = 0;
PImage ship;

PVector pos = new PVector( 150, 20 );
PVector speed = new PVector( 0, 0 );
PVector g = new PVector( 0, 1.622 );

float a = 0;
float acc = 0;
```

- 8. 此外，添加一个 keyPressed() 方法，使玩家能够更改火箭的旋转和加速度。

```java
void keyPressed() {
  if ( keyCode == LEFT ) {
    a -= 0.1;
  }

  if ( keyCode == RIGHT ) {
    a += 0.1;
  }

  if ( keyCode == UP ) {
    acc += 0.04;
  }

  acc = max( acc, 1/frameRate);
}
```

- 9. 现在，我们需要调整 drawShip() 方法，并使用旋转变量来改变飞船的旋转。

```java
void drawShip() {
  pushMatrix();
  translate(pos.x, pos.y);
  rotate(a);
  image( ship, -20, -40, 40, 75 );
  popMatrix();
}
```

- 10. 我们还需要调整 update() 方法，以使加速度和旋转角度影响飞船的速度和位置。

```java
void update() {
  PVector f = new PVector( cos( a+PI/2 ) * -acc,
  sin( a+PI/2 ) * -acc );
  if ( acc > 0 ) {
    acc *= 0.95;
  }

  PVector gDelta = new PVector( g.x / frameRate, g.y / frameRate);

  speed.add( gDelta );
  speed.add( f );
  pos.add( speed );

  if ( pos.x > landingX - 40 && pos.x < landingX + 40
  && pos.y > height-50 - ship.height/2 ) {
    pos.y = height - 50 - ship.height/2;
  } else if (pos.y > height - 20 - ship.height/2 ) {
    pos.y = height - 20 - ship.height/2;
  }
}
```

- 11. 我们的飞船现在可以转动，并对加速命令做出反应。现在，我们向 drawShip() 方法添加一些代码，以便在用户加速飞船时从推进器喷出火焰。

```java
void drawShip() {
  pushMatrix();
  translate(pos.x, pos.y);
  rotate(a);
  noFill();
  for ( int i=4; i >= 0; i--) {
    stroke(255, i*50, 0);
    fill(255, i*50, 20);
    ellipse( 0, 30, min(1, acc*10) *i*4, min(1, acc*10)* i*10);
  }

  image( ship, -ship.width/2, -ship.height/2, ship.width, ship.height );
  popMatrix();
}
```

- 12. 飞船在平台上着陆或撞到月球表面后，游戏只能通过重新启动草图才能再次进行。我们现在将通过向草图添加游戏状态来修复这一点。添加一个名为 state 的变量来存储当前状态，并定义三个名为 WAITING、RUNNING 和 FINISHED 的常量。我们将 state 变量初始化为 WAITING，并将 setup() 方法中的关卡生成代码提取到名为 reset() 的方法中。

```java
int[] moon;
int landingX = 0;
```

```java
PVector pos = new PVector( 150, 20 );
PVector speed = new PVector( 0, 0 );
PVector g = new PVector( 0, 1.622 );
```

```java
float a = 0;
float acc = 0;
```

```java
PImage ship;
```

```java
int WAITING = 1;
int RUNNING = 2;
int FINISHED = 3;
```

```java
int state = WAITING;
```

```java
void setup() {
  size(300, 300);
  ship = loadImage( "ship.png" );
  reset();
}
```

```java
void reset() {
```

