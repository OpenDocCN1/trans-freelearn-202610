

纬度是从北极到南极的坐标，范围从-90度到90度，赤道为0度。经度绕地球一周，范围从-180度到180度。经度零位于所谓的“本初子午线”，它穿过伦敦格林尼治皇家天文台——我猜这回答了谁发明了这个坐标系统的问题。

当我们想到一张地图时，通常北美和南美在左边，欧洲和非洲在中间，亚洲在右边。在这样的地图上，我们使用的地理坐标系的原点位于地图中心。

## 启动推进器

让我们在地图上画一些点：

- 1. 首先我们需要一张可以绘图的地图，因此要么使用我们上一个任务中用作纹理的世界地图，要么从 www.packtpub.com/support 下载。
- 2. 现在，我们打开草图，通过将纹理拖到草图窗口上，或使用 Sketch | Add File... 菜单来添加纹理。
- 3. 然后，我们添加一个 PImage 变量来存储地图，并在 setup() 方法中加载它。

```
PImage world;

void setup() {
  size(700,400);

  world = loadImage( "world.png" );

  Thread t = new Thread() {
    public void run() {
      state = LOADING;
      ipdatabase = parseIpDatabase( "hip_ip4_city_lat_lng.csv" );
      state = LOGFILE;
      String[] log = loadStrings( "access.log" );
      data = parseLogfile( log );
      state = GEOCODING;
      geoCode = data;
      state = DONE;
    }
  };

  t.start();
}
```

- 4. 在我们的 draw() 方法中，我们将时间轴移到窗口底部，并在其上方显示地图。

```
void draw() {
background(255);
image( world, 0, 0, width, 350 );

if ( state < DONE ) {
stroke( 0, 200 );
strokeWeight(2);
fill( 200, 200 );
rect( 25, 150, 650, 50 );
noStroke();
fill( 255 );
rect( 30, 155, map( count, 0, total, 0, 640 ), 40 );

fill( 0 );
String msg="";
if ( state == LOADING ) {
msg = "loading database ...";
} else if ( state == LOGFILE ) {
msg = "parsing logfile ...";
} else if ( state == GEOCODING ) {
msg = "geocoding ip adresses ...";
}

text( msg, 35, 190 );
} else if (state == DONE) {
noStroke();
fill(255);
rect(0,350,700,50);
stroke(0, 10);
long mints = data[0].timestamp;
long maxts = data[data.length-1].timestamp;
for ( int i=0; i<data.length; i++ ) {
float pos = map( data[i].timestamp, mints , maxts , 0, 700 );
line( pos, 350, pos,400 );
}
}
}
```

- 5. 运行草图。我们的地图应该如下面的截图所示：

![](img/5cb67baa7bc7778de360b14c00d1ab81_216_0.png)

- 6. 当我们的加载线程将状态设置为 DONE 时，我们将遍历日志文件行，并在地图上为每一对地理坐标绘制一个红点。

```
void draw() {
	background(255);
	image( world, 0, 0, width, 350 );

	if ( state < DONE ) {
		stroke( 0, 200 );
		strokeWeight(2);
		fill( 200, 200 );
		rect( 25, 150, 650, 50 );
		noStroke();
		fill( 255 );
		rect( 30, 155, map( count, 0, total, 0, 640 ), 40 );
	}

	fill( 0 );
	String msg="";
	if ( state == LOADING ) {
		msg = "loading database ...";
	}
	else if ( state == LOGFILE ) {
		msg = "parsing logfile ...";
	}
	else if ( state == GEOCODING ) {
		msg = "geocoding ip adresses ...";
	}

	text( msg, 35, 190 );
} else if (state == DONE) {
noStroke();
fill(255);
rect(0, 350, 700, 50);
stroke(0, 10);
long mins = data[0].timestamp;
long maxts = data[data.length-1].timestamp;
for ( int i=0; i<data.length; i++) {
float pos = map( data[i].timestamp, mins, maxts, 0, 700 );
line( pos, 350, pos, 400 );
}

stroke(255, 0, 0);
for ( int i=0; i < data.length; i++) {
if ( data[i].lat != null && data[i].lon != null ) {
float x = map( float( data[i].lon ), -180, 180, 0, width );
```

- 7. 运行草图。我们的地图现在应该布满红点，如下面的截图所示：

![](img/5cb67baa7bc7778de360b14c00d1ab81_217_0.png)

- 8. 为了让地图看起来更花哨，我们现在要用一个小图钉的矢量图形来替换红点。从 www.packtpub.com/support 下载 pin.svg 文件。

- 9. 通过将 .svg 文件拖到草图窗口上，或使用 **草图 | 添加文件...** 菜单将其添加到我们的草图中。

- 10. 现在，我们需要为图钉定义一个 PShape 变量，并在 setup() 方法中加载形状。

```
import java.text.SimpleDateFormat;

PImage world;
PShape pin;

void setup() {
size(700, 400);
pin = loadShape( "pin.svg" );
world = loadImage( "world.png" );

Thread t = new Thread() {
public void run() {
state = LOADING;
ipdatabase = parseIpDatabase( "hip_ip4_city_lat_lng.csv" );
state = LOGFILE;
String[] log = loadStrings( "access.log" );
data = parseLogfile( log );
state = GEOCODING;
geoCode( data );
state = DONE;
}
};
t.start();
}
```

- 11. 在我们的 draw() 方法中，我们用对 shape() 的调用来替换 point()。

```
void draw() {
...

stroke(255, 0, 0);

for ( int i=0; i < data.length; i++ ) {
if ( data[i].lat != null && data[i].lon != null ) {
float x = map( float( data[i].lon ), -180, 180, 0, width );
float y = map( float( data[i].lat), 90, -90, 0, height -50
);

shape( pin, int(x)-5, int(y) -15 );
}
}
}
}
```

- 12. 运行草图，在加载数据库并编码所有 IP 地址后，我们的地图现在如下面的截图所示：

![](img/5cb67baa7bc7778de360b14c00d1ab81_219_0.png)

## 任务完成 - 小型简报

我们当前任务的目标是使用上一个任务中创建的已地理编码的日志文件行来创建“红点热”。我们向草图中添加了一个世界示意图，其中大陆被绘制为黑色，海洋为白色。这张地图被用作我们上一个项目中创建的地球仪的纹理。

然后，我们在 draw() 方法中添加了一个循环来遍历 LogRow。对于每一行，我们检查数据库中是否找到了某些地理坐标，如果找到了就绘制一个红点。

为了让地图看起来更花哨，并且（至少有一点点）告别地图制作的“红点热”阶段，我们用一个小红图钉的矢量图形替换了红点。我们为它添加了一个 PShape 变量，并在 setup() 方法中加载了一个 SVG 图形。然后，图钉在 draw() 方法中使用 shape() 命令绘制。

## 交互式红点热

对于当前项目的这个最终任务，我们将使用为上一个项目创建的旋转霓虹地球仪。我们将用从地球仪伸出的黄色线条来替换我们的红图钉。这些黄色图钉也与我们基于着色器的滤镜配合得很好，该滤镜创建霓虹效果。

### 启动推进器

让我们将图钉添加到地球仪上：

- 1. 打开我们在本任务之前的任务中创建的草图以及上一个任务的最终草图。如果你没有跟上上一个任务，也可以从 www.packtpub.com/support 下载草图。

- 2. 在霓虹地球仪草图中，选择 Sketch | Add File... 菜单并打开 data 文件夹。

- 3. 将两个 GLSL 文件和图片添加到我们的红点热草图中。

- 4. 现在，为任务纹理添加一个 PImage 变量，为我们的地球仪添加一个 PShape 变量，并为模糊和边缘检测滤镜添加两个 PShader 变量。

```
PImage world;
PImage blue;
PImage night;
PShape pin;
PShape sphere;
PShader blur;
PShader edge;
LogRow[] data;
```

```
int LOADING = 0;
int LOGFILE = 1;
int GEOCODING = 2;
int DONE = 3;
int state = LOADING;
```

```
int count = 0;
int total = 0;
float a =0;
```

- 5. 我们现在创建一个名为 initGlobe() 的新方法，并从 setup() 方法中调用它。我们还将草图的大小更改为 400 x 450 像素，并启用 P3D 渲染模式。

```
void setup() {
size(400, 450, P3D);
pin = loadShape( "pin.svg" );
world = loadImage( "world.png" );

frameRate(25);
initGlobe();

Thread t = new Thread() {
    public void run() {
        state = LOADING;
        ipdatabase = parseIpDatabase( "hip_ip4_city_lat_lng.csv" );
        state = LOGFILE;
        String[] log = loadStrings( "access.log" );
        data = parseLogfile( log );
        state = GEOCODING;
        geoCode( data );
        state = DONE;
    }
};
t.start();
}
```

- 6. 将霓虹地球仪草图 setup() 方法中的初始化代码复制到新的 initGlobe() 方法中。

```
void initGlobe() {
    PGraphics g = createGraphics( 700, 349 );
    g.beginDraw();
    PImage tmp = loadImage( "world.png" );
    tmp.filter(INVERT);
    g.tint(100, 255, 0);
    g.image( tmp, 0, 0 );
    g.endDraw();

    world = createGraphics( 700, 349 );
    world = g.get(0, 0, 700, 349);

    bluemarble = loadImage( "bluemarble.jpg" );
    night = loadImage( "night.jpg" );

    sphere = makeSphere( 150, 5, world);

    edge = loadShader("edges.glsl");
    blur = loadShader("blur.glsl");
}
```

