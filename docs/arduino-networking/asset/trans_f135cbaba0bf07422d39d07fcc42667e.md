

## 硬件和软件要求

在硬件方面，你当然需要Arduino Ethernet扩展板和一个Arduino开发板，例如Arduino Uno。

你还需要一个传感器来测量一些数据。由于本书是关于如何使用Ethernet扩展板，而不是如何从传感器测量，你实际上可以选择任何你喜欢的传感器。

我使用了DHT11传感器，这是一个数字温湿度传感器。我为本章和书中许多章节选择了这个传感器，因为它非常便宜且易于与Arduino接口。除了DHT11传感器，你还需要一个4.7k欧姆电阻。

要记录数据，你需要一张可以插入Ethernet扩展板的MicroSD卡。我没有特定的品牌推荐，但我使用了一张SanDisk MicroSD卡，配合普通SD适配器（对我很有效），所以你也可以将其插入电脑以检查数据是否正确记录。另外，请确保SD卡使用FAT32格式格式化。

你还需要一块面包板和一些跳线来在传感器和Ethernet扩展板之间建立连接。

以下是本章使用的所有元件列表：

- Arduino Uno (https://www.adafruit.com/products/50)
- Arduino Ethernet扩展板 (https://www.adafruit.com/products/201)
- DHT11传感器 (https://www.adafruit.com/products/386)
- MicroSD卡 (http://www.amazon.com/SanDisk-Micro-SDHC-Memory-Adapter/dp/B0052MHQM6)
- 面包板 (https://www.adafruit.com/product/64)
- 跳线 (https://www.adafruit.com/product/758)

在软件方面，你首先需要的是用于与之前选择的传感器接口的库。我为这个项目选择了DHT11传感器。你需要从 https://github.com/adafruit/DHT-sensor-library 下载并安装DHT库。

安装Arduino库非常简单，只需将下载文件的内容解压到主Arduino文件夹的libraries文件夹中（如果尚未创建，则创建该文件夹）。

## 硬件配置

现在是时候为本项目设置硬件了。到此为止，如果你跟随本书的前几章操作，你应该已经将Arduino Ethernet扩展板插入Arduino开发板，并用一根以太网线将扩展板和互联网路由器连接起来。如果还没有，请务必这样做。

在本项目中，你需要连接的只有DHT11传感器和电阻。你可以通过查看下面的原理图来了解需要进行的不同连接概览：

![](img/b454031c864977fe997c5feb0b3f9b63_78_0.png)

首先，将DHT11传感器插入面包板。然后，使用4.7k欧姆电阻连接传感器的引脚1和2。

接下来是电源。将传感器的引脚1连接到Arduino 5V，引脚4连接到Arduino GND。最后，将DHT传感器的引脚2连接到Arduino引脚7。

最终，它应该看起来像下面的图片：

![](img/b454031c864977fe997c5feb0b3f9b63_79_0.png)

最后，只需将SD卡插入Ethernet扩展板上的SD卡插槽即可。

## 在SD卡上记录数据

让我们从本章的第一个主题开始——使用Ethernet扩展板集成的SD卡插槽在SD卡上记录数据。我们将在Arduino开发板上进行一些测量，并将每次测量记录到SD卡上的一个简单文本文件中。

然而，与第2章“将数据发送到Web服务器”的项目相比，我们实际上并不知道测量进行时的日期。在前一章中，我们只是使用了计算机的日期，但这里我们没有这些信息。要在Arduino开发板上获取时间，我们将采用另一种策略。我们将使用Ethernet扩展板连接到NTP时间服务器，在Arduino草图启动时获取一次时间，然后使用Time库从那里开始跟踪时间。

要了解有关Time库的更多信息，可以访问Arduino网站上的以下页面：
http://playground.arduino.cc/Code/Time

在SD卡上记录数据的过程首先是按如下方式包含正确的库：

```
#include "DHT.h"
#include <SD.h>
#include <Time.h>
#include <Ethernet.h>
#include <EthernetUdp.h>
#include <SPI.h>
```

我们还需要定义传感器类型以及传感器连接的引脚。这是通过以下代码行完成的：

```
#define DHTPIN 7
#define DHTTYPE DHT11
```

要定义Ethernet扩展板的MAC地址，请使用以下代码行：

```
byte mac[] = { 0x90, 0xA2, 0xDA, 0x0E, 0xFE, 0x40 };
```

要使用SD卡，你还需要定义chipSelect引脚，即Ethernet扩展板上的引脚4。这是通过以下行完成的：

```
const int chipSelect = 4;
```

现在，我们来处理NTP服务器。实际上，你可以使用多个NTP服务器（你可以查看Time库附带的TimeNTP示例中许多这些服务器的IP地址）。你也可以在此设置时区。要了解可以使用哪个NTP服务器以及设置哪个时区的更多信息，可以查看示例TimeNTP草图。此草图作为Time库内的一个示例提供。对于本项目，我使用了第一个可用服务器，时区编号为1，如下面的代码所示：

```
IPAddress timeServer(132, 163, 4, 101);
const int timeZone = 1;
```

要连接到此服务器，我们还需要定义一个Ethernet客户端。这一次，我们将使用一种与之前不同的协议，即UDP。UDP与TCP不同——它简单得多且更轻量。但是，它不能保证数据被正确传送，而TCP可以。你实际上不需要关心该协议的细节，因为Time库会处理一切。你可以保留默认的UDP端口，如下面的代码所示：

```
EthernetUDP Udp;
unsigned int localPort = 8888;
```

你还需要按如下方式创建DHT传感器的实例：

```
DHT dht(DHTPIN, DHTTYPE);
```

现在，在草图的setup()函数中，你需要定义以下负责为Ethernet扩展板获取IP地址的代码：

```
Serial.begin(9600);
if (Ethernet.begin(mac) == 0) {
// no point in carrying on, so do nothing forevermore:
while (1) {
Serial.println("Failed to configure Ethernet using DHCP");
delay(10000);
}
}
```

之后，我们将尝试初始化SD卡，如下面的代码所示：

```
Serial.print("Initializing SD card...");
pinMode(10, OUTPUT);
```

如果成功，我们将在串口打印一条消息，如下面的代码所示：

```
if (!SD.begin(chipSelect)) {
Serial.println("Card failed, or not present");
// don't do anything more:
return;
}
Serial.println("card initialized.");
```

我们还将按如下方式启动DHT传感器：

```
dht.begin();
```

最后，仍在setup()函数中，我们将打印Ethernet扩展板的IP地址并创建与NTP服务器的连接。请注意，此处我们正在调用一个名为getNtpTime的函数，该函数在草图的后面部分定义。由于这是Time库提供的函数，我们不会深入了解该函数的细节。以下是此部分的代码：

```
Serial.print("IP number assigned by DHCP is ");
Serial.println(Ethernet.localIP());
Udp.begin(localPort);
Serial.println("waiting for sync");
setSyncProvider(getNtpTime);
```

现在，在草图的loop()函数中，我们将首先使用以下代码从传感器测量温度和湿度：

```
float h = dht.readHumidity();
float t = dht.readTemperature();
```

其次，我们将按如下方式将这些测量值转换为字符串：

```
String temp = String((int) t);
String hum = String((int) h);
```

我们现在将使用Time库构建一个包含测量日期和时间的字符串。该库包含用于获取当前时间、日期或月份的函数，例如，这些值是根据最初调用NTP服务器计算得出的。例如，调用day()函数将返回当前日期。以下代码片段返回包含日期和时间的字符串：

```
String log_time = String(day()) + "/" +
String(month()) + "/" + String(year()) + " " +
String(hour()) + ":" + String(minute()) + ":" +
String(second());
```

然后，我们使用逗号作为不同字符串之间的分隔符，将此字符串与温度和湿度测量值组装起来，如下面的代码行所示：

```
String dataString = log_time + "," + temp + "," + hum;
```

我们现在将把这些数据写入SD卡。我们首先需要使用以下行打开卡以写入数据：

```
File dataFile = SD.open("datalog.txt", FILE_WRITE);
```

如果成功，我们将把dataString变量的内容写入名为datalog.txt的文件，如下面的代码所示：

```
if (dataFile) {
dataFile.println(dataString);
dataFile.close();
Serial.println(dataString);
} else {
Serial.println("error opening datalog.txt");
}
```

最后，我们将每10秒重复一次该操作，但你当然可以使用以下行修改此延迟：

```
delay(10000);
```

## 注意

