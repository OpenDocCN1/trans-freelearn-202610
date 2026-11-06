## 6 设置游戏服务器

## 

在树莓派上运行 Counter Strike 服务器会很棒，但由于运行此类游戏服务器的高要求，这是不可能的。主要是由于 RAM 不足，但一些服务器也需要强大的处理器为所有玩家提供低延迟性能，每个游戏最多可达 24 名玩家。

在本章中，你将了解由对特定类型游戏充满热情的人开发的开源游戏。这些游戏是逆向工程的、从头构建的，或仅仅因其简单性而变得流行。

## 更新到 Jessie

在撰写本书时，树莓派的主要发行版仍是 Debian Wheezy。Debian Jessie 大约在 2013 年 6 月为树莓派测试而添加，但已在桌面机器上使用。截至 2015 年 4 月，似乎已有树莓派的稳定版本。许多游戏服务器软件包仅在 Jessie 中更新，因为依赖项更新。

你可以将整个树莓派升级到 Jessie，但建议在官方镜像发布前不要这样做。相反，我们将在保持整个发行版在 Wheezy 稳定的同时，从 Jessie 安装选定的特定软件包。如果您已在使用 Jessie，可以跳过这些步骤。

## 选择性设置

我们可以将以下两行添加到 /etc/apt/sources.list：

```
deb http://mirrordirector.raspbian.org/raspbian/ jessie main contrib non-free rpi
deb http://archive.raspbian.org/raspbian jessie main contrib non-free rpi
```

下一步是告诉 aptitude 对正常更新使用 `wheezy` 仓库，并且你希望不时使用 Jessie。这些设置应输入到位于 `/etc/apt/preferences` 的文件中。如果文件不存在，请创建它：

```
Package: *
Pin: release n=wheezy
Pin-Priority: 900

Package: *
Pin: release n=jessie
Pin-Priority: 300

Package: *
Pin: release o=Raspbian
Pin-Priority: -10
```

添加这些设置并保存文件后，运行 `sudo apt-get update`。

我们将使用 `apt-get` 并带有一个额外开关来使用新软件包。你应该仅在建议时使用此开关。升级核心软件包可能导致不稳定行为。我们将使用的语法如下，其中 `<package>` 将被以下段落中所需的游戏软件包替换：

```
sudo apt-get -t jessie install <package>
```

## 游戏服务器

我们将只关注运行不需要桌面交互的游戏的服务器部分。相反，你在另一台计算机上运行客户端并连接到树莓派。一些客户端可以在 Pi 的 X 桌面上运行，它们在树莓派 2 上运行效果相当不错，但可能不太愉快。

## OpenTTD

**OpenTTD** 是由 MicroProse 制作的原版《运输大亨》的重新实现。这款游戏极具上瘾性，加入网络游戏可以给你额外的竞争优势。

你必须对物流、策略和实时模拟充满热情才能享受这款游戏。目标只是尽可能高效地控制火车和卡车，并在运输货物时获利。

## 安装 OpenTTD

最新软件包仅保留在 Jessie 上：

```
sudo apt-get -t jessie install openttd
```

在此期间将安装多个库。你可能会看到一个蓝色屏幕，询问你是否要重启某些服务以完成软件包安装。同意是安全的。这可能需要几分钟才能完成。

## 配置 OpenTTD

名为 `openttd.cfg` 的配置文件可以在 `~/.openttd` 找到。它包含几百个可根据偏好调整的设置，但其中一些重要设置如下：

- `lan_internet`: 这设置了 LAN 访问条件。对于 lan0，允许互联网，对于 lan1，仅使用 LAN。
- `server_name`: 这是用于在互联网上标识服务器的唯一名称。
- `server_advertise`: 我们将其设置为 `true`，以便它在服务器列表中可见。
- `server_bind_ip`: 我们将其设置为 `0.0.0.0`；这允许地址绑定到任何网络。
- `server_port`: 对此，我们保留默认值；这是其他人从互联网连接到网络的方式。

> 要允许人们从互联网连接，你应该在路由器中将 3979 和 3978 端口转发到树莓派。

使用以下选项启动服务器：

```
/usr/games/openttd -D
```

## 游玩 OpenTTD

你现在可以在其他计算机上安装并运行 OpenTTD，并邀请一些朋友加入你。

点击 **Multiplayer**，如果已为游戏转发端口，则选择 **Internet**。点击 **find**，将显示服务器列表。




## Freeciv

> "这是一款受人类文明历史启发的免费开源帝国建设策略游戏。"
> 
> – Freeciv

这款游戏让我想起 Sid Meier 的《文明》。它的用户界面非常相似，但一旦开始游玩，你会注意到游戏中内置了更多功能和可能性。

## 安装 Freeciv

我们可以从 Raspberry 仓库安装服务器：

```
sudo apt-get -t jessie install freeciv-server
```

在此期间会安装多个库。你可能会看到一个蓝色屏幕，询问你是否要重启某些服务以完成软件包安装。同意是安全的。这可能需要几分钟才能完成。

## 配置 Freeciv

默认设置足以让你立即游玩。你可以通过在控制台输入以下内容来启动服务器：

```
freeciv-server
```

服务器不应使用 sudo 命令或以 root 用户身份运行，而应以普通无特权用户身份运行。

这实际上会为你提供由 freeciv 服务器提供的另一个控制台。你可以通过输入命令来调整服务器设置。你可以尝试输入 help 并按 Enter。

如果你想将服务器发布到在线社区，你需要通过路由器将端口 5555 转发到 Raspberry Pi，然后使用额外的命令行 --meta 启动服务器。

## 游玩 Freeciv

你现在可以在电脑上安装并运行 `freeciv`。如果你运行的是服务器和客户端的相同版本，你将在局域网中看到它。

## OpenArena

**OpenArena** 是一款基于 Quake 3 引擎的免费第一人称射击游戏。它的目标是成为 Quake 3 的克隆版，但它用全新的内容替换了专有内容。

## 安装 OpenArena

服务器安装可能占用多达 500 MB。请确保有足够的可用空间，然后我们可以使用 Jessie 仓库安装游戏：

```
sudo apt-get -t jessie install openarena-server
```

在此期间会安装多个库。你可能会看到一个蓝色屏幕，询问你是否要重启某些服务以完成软件包安装。同意是安全的。这可能需要几分钟才能完成。

## 配置 OpenArena

如果你想在互联网上共享服务器，你应该在路由器上打开并转发 UDP 端口 27960 和 27950。查看位于 `/etc/openarena-server/server.cfg` 的设置：

- `set dedicated 2`: 我们在互联网上使用 2，而 1（默认）是局域网
- `sv_hostname "Raspberry Pi"`: 用于设置你的服务器名称
- `sv_master1 "dpmaster.deathmask.net"`: 设置保存列表的互联网服务器
- `sv_maxclients 16`: 这是允许连接的客户端的最大数量
- `capturelimit 8`: 用于设置 CTF 中的捕获限制
- `timelimit 15`: 用于设置时间限制
- `fraglimit 35`: 用于设置爆头限制
- `g_motd "This Pie is delicious!"`: 用于设置每日信息
- `g_gametype 0`: 用于将游戏类型设置为自由混战；也称为死亡竞赛

你可以在 OpenArena Wiki 上找到数百个额外设置。请记住服务器不得以 root 身份运行。

OpenArena 服务器作为服务运行。我们应该停止服务并在控制台上运行它，以确保一切正常运行。一旦你确认一切配置正确，你可以再次启动服务：

```
sudo service openarena-server stop
/usr/games/openarena-server +exec /etc/openarena-server/server.cfg
```

## 游玩 OpenArena

该游戏可以在 Windows、Mac OS X 或 Linux 上游玩。你应该前往网站获取特定平台的客户端。

## Minecraft

Minecraft 是一款沙盒独立游戏，你控制一个可以建造你想象中任何东西的人。有复杂的方块可以被使用或编程来执行各种服务器端计算。如果你想看到 Minecraft 的完整潜力，你应该访问 BitVegas。这比其他服务器更困难，因为我们需要安装和配置大量依赖项。

## 安装 Java Hard-Float

我们需要前往 https://jdk8.java.net/download.html，接受条款，并复制适用于 Linux ARM 32 位的链接。现在，我们需要下载文件，忽略任何证书问题：

```
sudo wget --no-check-certificate <URL>
```

然后，在控制台中输入以下行，并将 <filename> 替换为你下载的最新文件：

```
mkdir -p /opt
sudo tar zxvf <filename> -C /opt
sudo /opt/jdk????/bin/java -version
```

![](img/a8753cacc9d454dfc09eda1ed5012bd3_100_0.png)

你可以开始输入文件名然后按 Tab。这将自动补全文件名或为方便起见显示类似名称的列表。

如果你从版本命令行得到响应，那么你现在已在 Raspberry Pi 上安装了 Java Hard-Float。

## 安装 Minecraft 服务器

Minecraft 服务器由各个群体积极开发，有许多配置可供选择。Spigot 是一款高性能的 Minecraft 服务器实现，我们可以使用以下命令安装它：

```
cd /home/pi
sudo wget http://ci.md-5.net/job/Spigot/lastStableBuild /artifact/Spigot-Server/target/spigot.jar
```

```
### This is the command line use on Pi 2 1GB ram
sudo /opt/jdk1.8.0/bin/java -Xms512M -Xmx992M -jar /home/pi/spigot.jar nogui
```

```
#This is the command line use on Model B with 512 ram
sudo /opt/jdk1.8.0/bin/java -Xms256M -Xmx496M -jar /home/pi/spigot.jar nogui
```

第一次启动大约需要 10 分钟。一旦准备就绪，你可以从另一台机器本地登录并探索你的新世界。你应该稍后退出并为性能配置服务器。

## 配置 Minecraft

要允许互联网上的玩家连接，你需要从路由器打开并转发名为 25565 的端口到 Raspberry Pi。

这些设置似乎最适合在 Raspberry Pi 上运行。你可以根据需要进行调整：

```
server-name=Raspberry Picraft!
motd=Pie is Delicious
allow-flight=false
spawn-monsters=true
generate-structures=true
enable-query=false
enable-rcon=false
level-name=world
spawn-protection=16
online-mode=true
```

```
difficulty=1
gamemode=0
spawn-animals=true
```

设置游戏服务器

```
view-distance=4
level-seed=
hardcore=false
snooper-enabled=false
level-type=DEFAULT
pvp=true
texture-pack=
max-players=20
server-ip=
max-build-height=240
spawn-npcs=true
server-port=25565
white-list=false
generator-settings=
allow-nether=false
```

## 游玩 Minecraft

要连接到你的服务器，你需要在网络设置中输入你的 Raspberry IP 地址。如果你希望其他人从互联网加入，你应该阅读第 2 章《准备网络》，以了解如何设置动态 DNS。

## 摘要

在本章中，你学习了如何使用另一个非默认的 Jessie 仓库以及如何在 Raspberry Pi 上设置各种游戏服务器。你还学习了如何从 aptitude 安装 OpenTTD、Freeciv 和 OpenArena。我们介绍了如何使用 Java Hard-Float 设置高性能 Minecraft 服务器。

在下一章，我们将设置实时视频流。
