

| 端口 | I/O 引脚 | 标签/用途 |
|------|----------|-----------|
|      | 5 (GP5) | 撤防/布防蜂鸣器 (输出) |
|      | 6 (GP6) | 警报 LED (输出) |
|      | 7 (GP7) | 警报铃 (输出) |

## 为自己布防

术语布防和撤防是报警系统用语，分别指开启报警监控（布防系统）和关闭报警监控（撤防系统）。我们系统的区域 1 将与布防和撤防部分相连，因为它将连接到我们进出时使用的门上的传感器；这将是一个用于进出目的的特殊区域。

当我们设置报警时，我们需要一点时间离开房子。系统知道我们已离开物业的方式是通过监控出口区域，查看我们在允许的时间内是否打开了前门然后在我们身后关闭了前门。

同样，当我们返回时，我们会打开前门，但我们不希望报警立即响起——我们需要在给定时间内有机会撤防。我们将通过基于网络的控制面板或使用输入 GP0 上的某种开关来布防和撤防系统。

## 主配置文件

我们的系统将使用一个主配置文件来告诉它所有设置和连接方式。此配置文件将同时被 Web 控制面板和主报警控制脚本使用，以便两个子系统可以“相互通信”。让我们使用初始设置创建该文件。

设置文件将存储在我们将在第 9 章《整合一切》中创建控制脚本的同一位置，即在文件夹 /etc/pi-alarm 中。因此，让我们创建此文件夹，并赋予其执行权限，以便我们的脚本可以运行：

```text
$ cd /etc
$ sudo mkdir pi-alarm
$ sudo chmod 777 pi-alarm
```

我们现在将在此文件夹中创建供系统使用的主配置文件：

```text
$ cd pi-alarm
$ sudo nano alarm.cfg
```

如前所述，您不必在树莓派上使用 Nano 创建文件——您可以在桌面计算机上创建它们，然后使用 SCP 将它们传输到您的 Pi。

```text
# ALARM MASTER CONFIG FILE #

#Number of zones in the system
NUM_ZONES=8

#Display labels for each zone
ZONE_LABEL_1="Zone 1 - Entry/Exit"
ZONE_LABEL_2="Zone 2"
ZONE_LABEL_3="Zone 3"
ZONE_LABEL_4="Zone 4"
ZONE_LABEL_5="Zone 5"
ZONE_LABEL_6="Zone 6"
ZONE_LABEL_7="Zone 7"
ZONE_LABEL_8="Zone 8"

#Zones that are enabled
#Set to 0 to Disable or 1 to Enable
ZONE_ENABLE_1=1
ZONE_ENABLE_2=1
ZONE_ENABLE_3=1
ZONE_ENABLE_4=1
ZONE_ENABLE_5=1
ZONE_ENABLE_6=1
ZONE_ENABLE_7=1
ZONE_ENABLE_8=1

SYSTEM_ARMED=0

#Zone status
#Set to 1 if zone is triggered
ZONE_STATUS_1=0
ZONE_STATUS_2=0
ZONE_STATUS_3=0
ZONE_STATUS_4=0
ZONE_STATUS_5=0
ZONE_STATUS_6=0
ZONE_STATUS_7=0
ZONE_STATUS_8=0
```

alarm.cfg 文件

## 创建网页

我们的基于 Web 的控制面板将是一个由 **移动优化** 的单个 PHP 驱动的 HTML5 网页。HTML5 是网页的最新标记标准，受大多数现代智能手机和浏览器支持。我们还将创建一个 **级联样式表 (CSS)**，使我们的页面在移动设备上看起来还算过得去。

要创建 Web 文件，我建议您在桌面计算机上使用类似出色的 Notepad++ 之类的工具，而不是直接在树莓派上操作。或者，如果您是一位经验丰富的 Web 开发人员，您可能已经有了您选择的 IDE。

### 控制面板 HTML 模板

我们要做的第一件事是创建一个 HTML 文件，我们可以在将其放入 PHP 文件以使其与系统交互之前用来测试布局。这样在 PHP 脚本干扰之前，我们就可以更轻松地调整我们想要的外观。

> 这不是 Web 开发教程——关于该主题有很多书籍——但我希望代码足够清晰，让您弄清楚发生了什么。我要向您展示的代码是完全可用的，因此您可以直接使用我提供的内容而无需做更多。希望它也能让您的控制面板看起来不错！

以下标记为您提供了一个基本控制面板，其中包含我们 8 个区域的状态、主布防和撤防开关，以及启用或禁用任何区域的开关。

代码的 `<head>` 部分包含一些 `<meta>` 标签，帮助移动设备知道这是一个移动友好的网站。在主要的 `<body>` 标记中，我们为每个区域都有一个部分，其中包含区域名称和开/关开关。每个区域都在其容器中，以便我们也可以突出显示需要我们注意的特定区域，例如它被触发时。

您可以在 `chapter 7` 的代码文件夹中 `alarm-panel.html` 文件中找到我们控制面板的完整 HTML5 标记。

### 赋予它一些样式

目前，这个页面看起来并不太好（事实上，它看起来很糟糕，像是 1990 年代的东西）；它不特别适合移动设备，并且肯定会失败 *香肠测试*。因此，我们将应用一些样式使其看起来不算太差。尽管前面的标记包含对 CSS 文件的引用——我们还没有创建该文件——所以这就是我们页面目前的样子（正如我所说：它看起来很糟糕）：

![](img/631e8b200cce187082ee46b0eb1681b3_397_0.png)

没有样式的 Web 控制面板

以下 CSS3 标记专门为我们的控制面板设计，它使它看起来相当不错，同时也使其在 **触摸屏** 移动设备上可用。CSS 非常长，看起来很 overwhelming，但您无需对其做任何事情，也无需理解它（如果您不想的话）——您唯一需要知道的是它为现代浏览器和智能手机设计，因此不要期望它在 Internet Explorer 7 上工作，甚至可能 IE9 也不行！

本质上，它包含以下内容的样式：

- 为我们的移动布局准备浏览器
- 我们的文本和区域区域
- 创建很酷的开关而不是无聊的复选框
- 当我们需要时使某个区域闪烁

```css
/* Clear browser margin and padding defaults */
body, div, dl, dt, dd, ul, ol, li, h1, h2, h3, h4, h5, h6, pre, form,
fieldset, input, textarea, p {
margin:0;padding:0;-webkit-text-size-adjust:none;
}

body {
    background: #ffffff;
    color: #4A5651;
    font-family: "Trebuchet MS", Helvetica, sans-serif;
    font-size:10px;
    height: 100%;
    padding:0;
    margin:0 auto;
    max-width:320px;
    min-width:240px;
    text-align: left;
    width:100%;
    -webkit-box-shadow: 0px 20px 40px 0px rgba(0,0,0,0.50);
    -moz-box-shadow: 0px 20px 40px 0px rgba(0,0,0,0.50);
    box-shadow: 0px 20px 40px 0px rgba(0,0,0,0.50);
}

p, .zoneLabel {
    font-size:16px;
    margin:5px;
    line-height:1.4;
    color:#4A5651;
}

#header h1 {
    font-size:20px;
    line-height:40px;
    margin:0;
    padding:0 0 0 15px;
    text-align:center;
    text-overflow: ellipsis;
    font-weight:bold;
}

.zoneControl, .masterControl{
  border-bottom:1px solid #dddddd;
  margin-top:5px;
  margin-bottom:0px;
  padding:5px;
  display:block;
  width:100%;
}

.zoneLabel {
  font-weight:bold;
  text-overflow:ellipsis;
}

input[type="submit"] {
  border: none;
  background-color: #0b70cc;
  color: white;
  height: 32px;
  display: block;
  padding: 4px 7px;
  float: left;
  border-radius: 8px;
  position: relative;
  bottom: 1px;
  margin-left: 4px;
  text-align: center;
}
input[type="submit"]:hover {background-color: #b2ceec;color: #0b70cc;border: none;border: 1px solid #b2ceec;}

/* Flashing animation */
@-webkit-keyframes flash{0%, 50%, 100% {opacity: 1;} 25%, 75% {opacity: 0;}}
@keyframes flash {0%, 50%, 100% {opacity: 1;} 25%, 75% {opacity: 0;}}
.flash {
  -webkit-animation-name:
  flash;animation-name:
  flash;color:#f00000;
}

.animated {
  -webkit-animation-duration: 1s;
  animation-duration: 1s;
  -webkit-animation-fill-mode: both;
  animation-fill-mode: both;
  animation-iteration-count: infinite;
  -webkit-animation-iteration-count:infinite;
}

/*
  ON/OFF SWITCH STYLES
  The rather cool On/Off switch styling was generated on
  https://proto.io/freebies/onoff/
*/
.onoffswitch {
  position: relative;
  width: 90px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
}

.onoffswitch-checkbox {
  display: none;
}

.onoffswitch-label {
  display: block;
  overflow: hidden;
  cursor: pointer;
  border: 2px solid #FFFFFF;
  border-radius: 20px;
}

.onoffswitch-inner {
  display: block;
  width: 200%;
  margin-left: -100%;
  transition: margin 0.3s ease-in 0s;
}

.onoffswitch-inner:before, .onoffswitch-inner:after {
  display: block;
  float: left;
}
```
