
从第 10 步开始，我们删除了闪烁代码，并用调用 `Serial` 对象的 `println()` 方法来替代它，该方法使用内置串口将电阻值发送到计算机。要读取这些值，我们使用了 Arduino IDE 提供的串行监视器。

在第 16 步，我们开始编写一个 Processing 应用程序，它监听串行端口而不是串行监视器，并使用 Processing 的 `serial` 库显示其在串行端口上读取的值。

## 机密情报

Arduino 从可变电阻发送的值范围是 0 到 1,023。通常，我们期望旋钮左侧的值为零，右侧为最大值。如果您的控制器发送的值正好相反，您有两种方法来修复此问题。在硬件方面，您可以切换连接到 Arduino 开发板上 **Gnd** 和 **5V** 的导线。在软件方面，您可以通过用最大值减去该值来计算值：

```
val = 1023 - val;
```

如果您已经焊接了电路，软件方面尤其有帮助。

## 构建您的控制器

在本次任务的第一个任务中，我们学习了如何将可变电阻连接到 Arduino 并使用串行端口将其值发送到计算机。我们的下一个任务是扩展这个基本电路并再添加两个电阻，因为我们想在下一个任务中使用它们来控制 Smilie-O-Mat 草图的所有面部参数。

我们还将向电路中添加一个按钮，并学习如何检测点击事件并将其发送到我们的草图。我们将稍微扩展通信协议，并编写一个新的 Processing 草图，该草图将解析我们的 Arduino 生成的消息并显示我们发送的值。

## 启动推进器

让我们构建我们的控制器：

- 1. 对于此任务，我们需要扩展电路并再添加两个可变电阻。所以让我们先将 5V 引脚连接到面包板的一个外部电源轨，将 Gnd 引脚连接到另一个外部电源轨。如下图所示，将三个电阻连接到面包板：

![](img/5cb67baa7bc7778de360b14c00d1ab81_131_0.png)

- 2. 将每个电阻的左外侧引线连接到 5V 电源轨，将另一侧连接到 Gnd，如下图所示：

![](img/5cb67baa7bc7778de360b14c00d1ab81_132_0.png)

- 3. 现在将 Arduino 引脚（模拟输入 0、1 和 2）连接到电阻的中间引脚。您的布线应如下图所示：

![](img/5cb67baa7bc7778de360b14c00d1ab81_133_0.png)

- 4. 在我们的 loop() 方法中，我们需要复制读取值并将其发送到串行端口的代码，以读取另外两个端口：

```
int a = 0;
int lastA = a;

int b = 0;
int lastB = b;

int c = 0;
int lastC = c;

void setup() {
  Serial.begin( 9600 );
}

void loop() {
    a = analogRead( 0 );
    if ( abs( a - lastA ) > 10 ) {
        Serial.println( a );
        lastA = a;
    }
    b = analogRead( 1 );
    if ( abs( b - lastB ) > 10 ) {
        Serial.println( b );
        lastB = b;
    }
    c = analogRead( 2 );
    if ( abs( c - lastC ) > 10 ) {
        Serial.println( c );
        lastC = c;
    }
}
```

- 5. 编译代码并将其上传到 Arduino。

- 6. 现在运行我们为第一个任务“连接您的 Arduino”创建的 Processing 草图，并转动电阻的旋钮。我们看到值在变化，但我们的 Processing 草图无法识别是哪个电阻发送了该值。所以，切换回 Arduino IDE，让我们在值前面添加字母 A、B 或 C：

```
void loop() {
    a = analogRead( 0 );
    if ( abs( a - lastA ) > 10 ) {
        Serial.print( "A" );
        Serial.println( a );
        lastA = a;
    }
    b = analogRead( 1 );
    if ( abs( b - lastB ) > 10 ) {
        Serial.print( "B" );
        Serial.println( b );
        lastB = b;
    }
    c = analogRead( 2 );
    if ( abs( c - lastC ) > 10 ) {
        Serial.print( "C" );
        Serial.println( c );
        lastC = c;
    }
}
```

- 7. 编译并上传代码到 Arduino。

- 8. 现在运行 Processing 草图并转动旋钮。每个旋钮控制我们 Processing 草图中的一个对应变量。这里可以看到输入 B 的值已更改：

![](img/5cb67baa7bc7778de360b14c00d1ab81_135_0.png)

- 9. 要完成控制器的电路，我们需要添加一个按钮。将按钮的两个引线连接到面包板，并添加 10 kOhm 电阻，如下图所示：

![](img/5cb67baa7bc7778de360b14c00d1ab81_135_1.png)

- 10. 现在从 Gnd 线到您刚刚添加的电阻建立一个连接，并从 5V 线到按钮的第二个触点建立一个连接。

- 11. 将 Arduino 开发板的数字 2 引脚连接到电阻和按钮之间，如下图所示：

![](img/5cb67baa7bc7778de360b14c00d1ab81_136_0.png)

- 12. 您 Arduino 的数字引脚可以配置为输入和输出，因此将以下高亮代码行添加到 Arduino 草图的 setup() 方法中，以将引脚 2 配置为用作输入：

```
void setup() {
  pinMode( 2, INPUT );
  Serial.begin( 9600 );
}
```

- 13. 现在添加一个变量来存储按钮的上一个状态，并将其初始化为 LOW：

```
int button = LOW;
int buttonLast = LOW;
```

- 14. 在我们的 `loop()` 方法中，我们将读取按钮的当前状态并将其与我们保存的上一个值进行比较。如果按钮状态从 *pressed* 变为 *released*，我们将向串行总线发送一条消息：

```
void loop() {
  a = analogRead( 0 );
  if ( abs( a - lastA ) > 10 ) {
    Serial.print( "A" );
    Serial.println( a );
    lastA = a;
  }

  b = analogRead( 1 );
  if ( abs( b - lastB ) > 10 ) {
    Serial.print( "B" );
    Serial.println( b );
    lastB = b;
  }

  c = analogRead( 2 );
  if ( abs( c - lastC ) > 10 ) {
    Serial.print( "C" );
    Serial.println( c );
    lastC = c;
  }

  button = digitalRead( 2 );
  if ( buttonLast == HIGH && button == LOW ) {
    Serial.println( "click" );
  }

  buttonLast = button;
}
```

- 15. 编译并上传代码到 Arduino，然后切换到 Processing。

- 16. 运行我们的 Processing 草图，转动旋钮，并按下按钮。每个旋钮现在发送自己的消息，当按钮被按下时，我们在 Processing 端收到一条消息。按下按钮时，我们的 Processing 草图看起来如下：

![](img/5cb67baa7bc7778de360b14c00d1ab81_137_0.png)

- 17. 您当前的布线看起来应该如下图所示：

![](img/5cb67baa7bc7778de360b14c00d1ab81_138_0.png)

## 任务完成 - 小型简报

在我们当前任务的这个任务中，我们创建了 Smile-O-Mat 控制器所需硬件的原型版本。我们添加了三个可变电阻，并在第 8 步中定义了一个协议，通过串行端口将这些输入的值发送到计算机。该协议还允许我们检测哪个输入旋钮已被转动。

从第 11 步开始，我们扩展了电路并添加了一个按钮。我们还扩展了发送到计算机的消息，添加了点击消息。当按钮值从 pressed 变为 released 时，将传输此消息，我们将在下一个任务中使用它来触发我们的 tweet () 方法。

## 改变您的面容

我们当前任务的任务是将我们刚刚创建的原型硬件控制器集成到 Smile-O-Mat 控制器中。我们将扩展程序以解析基于 Arduino 的控制器通过串行端口发送的消息，并调整滑块以匹配硬件旋钮的值。当我们的控制器的用户按下开发板上的按钮时，我们将调用 tweet () 方法。

## 启动推进器

让我们使用控制器更改我们的笑脸：

- 1. 打开我们上一个任务中的 Smile-O-Mat 草图。

- 2. 要访问串行端口，我们需要导入串行库，因此请使用 草图 | 导入库 ... | 串行 菜单来导入它。

- 3. 现在向您的草图添加一个 Serial 对象：

```
import twitter4j.*;
import processing.serial.*;

Serial port;

Twitter twitter;
String[] tweets = {
"I feel like so #SmilieOMat",
"I currently feel like this #SmilieOMat",
"This is how I feel #SmilieOMat"
}
...
```

- 4. 在 setup() 方法中，我们需要初始化串行端口。请务必更改串行端口的端口名称，就像在本次任务的第一个任务中所做的那样：

```
void setup() {
size( 300,390 );
smooth();
background( 255 );
colorMode( HSB );
textFont( createFont( "Georgia", 20 ) );
initTwitter();
port = new Serial( this, "/dev/ttyUSB1", 9600 );
}
```

- 5. 我们需要解析来自串行端口的消息；因此让我们添加 serialEvent() 方法，以便在每次有消息可用时收到通知。我们正在检查获取的字符串的首字母，如果它以大写字母 A、B 或 C 开头，那么我们将解析字符串的其余部分并将其存储在面部参数的控制变量中：

```
void serialEvent( Serial p ) {
String in = p.readStringUntil( '\n' );
if ( in != null ) {
if ( in.charAt( 0 ) == 'A' ) {
eye = int(in.substring( 1,in.length() - 2 ) );
```
