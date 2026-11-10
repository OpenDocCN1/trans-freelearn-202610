
## 神奇魔镜

魔镜魔镜在墙上，今天天气会怎样？我应该带伞吗？

![](img/d8322379620245fcd21eccc3855b8c78_55_0.png)

BRADLEY MELTON
一名网络工程师和 aspiring ‘专业极客’，他喜欢使用并赞颂树莓派。
imgur.com/A4kjx7w

### Quick Facts

- 项目使用 Google Code 制作
- 制作框架花了一周时间，代码花了几天
- 这是布雷德利的首个创意 Pi 项目
- 过去，他在家里的 Cisco 网络实验室中使用过树莓派
- 在线社区在使其正常工作方面提供了很大帮助

铁人电影以其描绘近乎触手可及的技术方式激励了人们令人惊叹。看着有人俯瞰阳光明媚的马里布，同时天气信息直接显示在他们面前，这是一个很棒的视觉效果。这并不是完全全新、从未见过的技术——汽车很久以前就开始将抬头显示器投影到挡风玻璃上了——但它从未在大型商业大片中流行过。虽然我们似乎还没有完全达到托尼·斯塔克卧室中令人难以置信的玻璃技术的阶段，但显然我们已经足够接近让镜子以同样的方式工作，至少布雷德利·梅尔顿用他的魔镜实现了这一点。

“It’s called a ‘Magic Mirror’, but a more accurate name would be a ‘Smart Mirror’,” Bradley tells us. “It’s a mirror that displays the information you need to know at a quick glance: the time, the date, the weather, and of course a compliment!”

这并不是同类中的第一面镜子，布雷德利承认他从 Michael Teeuw 的一个先前项目中汲取了一些灵感（有关更多细节，请参阅 Michael 的博客：magpi.cc/1PzFbWa），他借鉴了这个概念并将其简化到更适合初学者的水平，以便自己能更多地学习网页开发。

“Plus, because I know what each and every function does and how it works, it makes it easier to fix bugs as well as make improvements in the future.”

它是一面额外实用的镜子，因为它显示了从互联网获取的最新天气信息

它看起来像一面镜子，但实际上是一个巧妙的伪装——一个显示器，外加一些双面玻璃并被框架覆盖

![](img/d8322379620245fcd21eccc3855b8c78_55_1.png)

当然，一面镜子也应该能够反射你的形象并确保你看起来很棒

### A TECH REFLECTION

![](img/d8322379620245fcd21eccc3855b8c78_56_0.png)

![](img/d8322379620245fcd21eccc3855b8c78_56_1.png)

![](img/d8322379620245fcd21eccc3855b8c78_56_2.png)

### >STEP-01 Turn the mirror on

打开显示器会激活树莓派，其电源通过镜子本身上的 USB 端口供电。它启动到 Chromium 网络浏览器，该浏览器显示在屏幕上。

### >STEP-02 Getting the data

天气信息取自 Yahoo，使用 simpleWeather.js jQuery 插件抓取，然后在显示器上显示天气。时间也会显示出来，同时还有一句赞美。

### >STEP-03 Keep me updated

一切都按计时器工作，因此构成显示器的浏览器窗口中的天气和时间每 15 秒更新一次，而赞美每 30 秒更新一次。

而不是使用真正的镜子并在其上投影数据，魔镜使用的是一个宽屏显示器，其被置于纵向模式并在上方放置了丙烯酸双面镜。通过适当的照明和显示器调整，它可以反射得足够好以用作镜子，同时显示天气数据。电子设备相当简单：它只是一个通过 HDMI 连接到镜子的 Pi、一个用于检索在线数据的 WiFi 适配器，以及一根连接到显示器的 USB 电缆，这也是它获取电源的方式。为了收尾，布雷德利制作了一个木质框架覆盖在边框上，使整个装置伪装得更好一些。

“It’s not very complex,” Bradley informs us. “As long as you have a little bit of carpentry or DIY skill to build the frame and have a basic understanding of how to program, you should be able to build this. I have never used JavaScript or CSS before this project, and I only had a little bit of experience with HTML, but this webpage is built almost entirely from JavaScript and CSS.” At the time of writing, the mirror has been running for a few weeks without any problems. It seems like Bradley wants to include some holiday-themed extras to it, starting with some spooky additions for the Halloween just past. We’re hoping he will add some jollier ones for the Christmas period.

> 而不是使用真正的镜子并在其上投影数据，魔镜使用的是宽屏显示器

![](img/d8322379620245fcd21eccc3855b8c78_56_3.png)

上方 在万圣节对着镜子说三次“Bloody Mary”以触发语音激活的惊吓

## ADRIAN ATWOOD

爸爸、工程师、修补匠、木工、borker 和总是建造奇怪项目的疯狂爱好者。
buildxyz.xyz

![](img/d8322379620245fcd21eccc3855b8c78_57_0.png)

![](img/d8322379620245fcd21eccc3855b8c78_57_1.png)

使用代码打开系统并设置计时器，给予你完全的家长控制

一个标准 LCD 屏幕内置于这个漂亮的机柜中，并从树莓派显示 Kodi 或 Emulation Station

众多 USB 端口可用于控制器和 USB 存储驱动器；不过，媒体也可以通过网络访问

### Quick Facts

- 建造耗时 90 小时
- 这是 Adrian 的第一个 Pi 项目
- 已安装 Scratch 以鼓励孩子们编程
- 他很快就会添加无线 PS2 控制器
- Adrian 还有更多酷炫的 Pi 计划——请关注他的博客

## RPIKIDS

不是一部衍生周六早晨卡通片，RPiKids 是为 Adrian Atwood 的孩子们打造的全新娱乐系统

我们都见过为孩子娱乐而设的古怪家长控制方法。我们最喜欢的是为 PlayStation 供电的健身自行车，这可能看起来很残酷，但至少意味着他们能玩多久有一个截止点。最好的方法仍然是最简单的：一个密钥代码和一个计时器，这样孩子们就可以做些其他事情，而不是整天盯着屏幕，从而腾出电视让你可以一口气看完《童话镇》第三季。随着树莓派成为运行娱乐中心更强大的选择，难免会有勇敢的黑客为自己的孩子制作终极版本。认识一下 RPiKids，由 Adrian Atwood 创建。

“RPiKids is an all-in-one kids’ entertainment centre with integrated parental control features,” Adrian explains. “My kids can enjoy 16-bit games, watch movies and more, provided an adult has granted them access by entering a password and setting the timer.”

RPiKids 是一种硬件组合；系统本身由树莓派供电，负责娱乐方面的 Kodi、Emulation Station 等。密码和计时器由 Arduino Uno 使用旋转编码器处理（想想银行保险箱），它可以作为按钮按下以确认选择。所有这些都内置在一个外观精美、蒸汽朋克风格的机箱中，由 Adrian 本人手工用木材制作，并配有 3D 打印细节。

前面有 USB 访问端口用于连接控制器和便携式媒体存储，硬件可见于可拆卸的透明 Perspex 面板后面。虽然一切看起来都非常令人印象深刻，但 Adrian 声称整体构建水平为“中级”。

“I will say the Raspberry Pi configuration was very straightforward and a beginner can mirror my configuration,” he tells us. “Coding the firmware for the Arduino parental controls, and having the Arduino and Pi 2 communicate with each other, was a little more complex. The case is no problem if you’re into woodworking and auto body repair. I have access to a 3D printer and laser cutter and enjoy CAD modelling, but those parts could be contracted out or replaced.”

### ENTERTAINING THE KIDS

驱动中心的设备都可以通过这块 Perspex 板看到——便于维护和好奇心强的孩子们观察

![](img/d8322379620245fcd21eccc3855b8c78_58_0.png)

![](img/d8322379620245fcd21eccc3855b8c78_58_1.png)

### >STEP-01 Dial it up

使用数字编码器和 NeoPixel LED 环，用户（家长）输入组合来解锁系统，以便孩子使用。

![](img/d8322379620245fcd21eccc3855b8c78_58_2.png)

### >STEP-02 Set a time

解锁后，编码器可用于在系统上设置计时器，从十分钟到无限期。一旦选择，Arduino 就会让树莓派开机。

![](img/d8322379620245fcd21eccc3855b8c78_58_3.png)

### >STEP-03 Shut it down

时间用完时（或按下关机按钮时），Arduino 部分会向 Pi 发出关机命令。Pi 然后安全地关闭所有设备，Arduino 稍后会切断电源。

> RPiKids 是一个集成了家长控制功能的一体化儿童娱乐中心

虽然他预计会出现“各种”故障，但该系统对他和他的孩子们来说工作得非常好，只需要对与编码器一起使用的 NeoPixel 环的亮度进行少量调整。

One thing Adrian recommends for anyone trying to replicate the project is an upgrade to the sound output: “The Pi definitely needed a DAC (I²S) sound card because there was a lot of hissing and artifacts with the onboard sound.”

孩子们非常喜欢它。虽然它是一个娱乐中心，但他还在锁机制中编程了一个有意的 安全漏洞，希望他的孩子们能从中学习到有关安全系统的知识。我们祝他们好运，希望他们能弄清楚如何解锁“无限模式”！
