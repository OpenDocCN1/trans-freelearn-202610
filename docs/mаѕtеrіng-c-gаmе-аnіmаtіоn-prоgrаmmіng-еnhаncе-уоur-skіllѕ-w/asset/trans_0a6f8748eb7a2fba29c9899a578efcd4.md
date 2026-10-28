

## 附加资源
- Pac-Man 中的幽灵导航：https://gameinternals.com/understanding-pac-man-ghost-behavior
- Dijkstra 算法：https://graphicmaths.com/computer-science/graph-theory/dijkstras-algorithm/
- Quake III Arena 的区域感知系统：http://www.kbs.twi.tudelft.nl/docs/MSc/2001/Waveren_Jean-Paul_van/thesis.pdf
- 导航网格：https://www.gamedev.net/tutorials/programming/artificial-intelligence/navigation-meshes-and-pathfinding-r4880/
- Shakey 机器人：https://www.sri.com/hoi/shakey-the-robot/
- A* 简介：https://www.redblobgames.com/pathfinding/a-star/introduction.html
- 使用导航网格的 A*：https://medium.com/@mscansian/a-with-navigation-meshes-246fd9e72424
- Unreal Engine 中的导航网格：https://dev.epicgames.com/documentation/en-us/unreal-engine/basic-navigation-in-unreal-engine
- 在 Unreal Engine 项目中实现 A*：https://www.youtube.com/watch?v=xak129fupCA
- 在 Trackmania 中训练无敌 AI：https://www.youtube.com/watch?v=Dw3BZ6O_8LY
- Recast Navigation：https://github.com/recastnavigation/recastnavigation
- 平滑路径优化：https://www.gameaipro.com/GameAIPro3/GameAIPro3_Chapter20_Optimization_for_Smooth_Paths.pdf
- 更逼真的寻路：https://www.gamedeveloper.com/programming/toward-more-realistic-pathfinding

## 在 Discord 上加入我们的社区

加入我们社区的 Discord 空间，与作者和其他读者进行讨论：
https://packt.link/cppgameanimation

![](img/3cfb9832c943f5f7366fd104b5255971_479_0.png)

## 14

## 创建沉浸式交互世界

欢迎来到第 14 章！在上一章中，我们为实例添加了寻路和导航功能。我们首先简要概述了计算机游戏中导航方法。然后我们探索并实现了 A* 算法，以在实例位置和虚拟世界中的某一点之间找到路径。接下来，我们增强了模型以作为导航目标，并扩展实例使其能够找到通往其中一个目标的路径。在本章末尾，我们添加了实例跟随路径并到达所需目标的能力。

在本章中，我们将探索不同的想法来增强示例代码，以获得更强的沉浸感和视觉质量。我们将从听觉方面着手，介绍为应用程序添加音效和音乐的工具和方法。还将实现一个简单的音频管理器类来播放音效和背景音乐。为了不仅取悦耳朵，还取悦眼睛，我们将收集一些关于如何增强视觉外观的想法，并将其中两种图形增强功能实现到应用程序中。我们将以本章结束本书，探讨昼夜和天气变化对虚拟世界的影响，并在应用程序中添加一个基本的昼夜循环。

在本章中，我们将涵盖以下主题：
- 添加音效和背景音乐
- 增强视觉效果
- 通过昼夜和天气扩展沉浸感

## 技术要求

本章的示例代码可在 chapter14 文件夹中找到：用于 OpenGL 的 01_opengl_ideas 子文件夹和用于 Vulkan 的 02_vulkan_ideas 子文件夹。

## 添加音效和背景音乐

运行第 13 章的示例代码提供了许多功能，例如面部动画、关卡加载、碰撞检测以及寻路和导航。但遗憾的是，屏幕上的所有动画都在死一般的寂静中运行。没有滴滴声或嗡嗡声，没有声音，也没有音乐。只有寂静。

但声音和音乐是游戏的重要组成部分，原因非常充分。无论你是在享受 Minecraft 的宁静草地还是深邃洞穴，沉浸在《超级马里奥》系列欢快的音乐中并听到动作的音效作为听觉反馈，还是在游荡于《死寂》或《寂静岭》系列令人毛骨悚然的世界中，仔细聆听环境音以了解周围情况，或是驾驶汽车伴随着《极品飞车》系列等赛车游戏的澎湃音乐和厚重音效——没有音乐和声音，游戏就无法为玩家提供同样的体验。

要添加声音输出，我们可以包含一个可自由获取的声音库，让我们能够轻松播放音效或音乐。让我们先看看一些库。

## 使用音频库

大多数音频库是用 C 编写的，但可以找到 C++ 绑定，以面向对象的方式封装特定于操作系统的底层函数调用。也可以只使用 C 函数并构建自定义抽象，类似于 GLFW、OpenGL 和 Vulkan。

## Simple DirectMedia Layer

Simple DirectMedia Layer（SDL）是一个跨平台库，用于计算机的多媒体硬件组件。SDL 管理音频，可作为窗口功能、图形上下文和输入处理的框架，如 GLFW。此外，还有多个官方库提供导入和导出图像、自定义网络以及字体渲染以在屏幕上显示文本的支持。

## OpenAL

OpenAL 是一个跨平台库，专注于多通道、三维音频。通过使用 3D 音频，声音可以被建模为在玩家前方或后方，而不仅仅是左右，从而加深沉浸感。

## PortAudio

PortAudio 是另一个跨平台音频库。它针对实时音频播放和录制。如果 SDL 和 OpenAL 的范围对项目来说过大，而目标只是播放一些音频，则可以使用 PortAudio。

## FMOD

尽管 FMOD 是一个专有的声音引擎，但它可以被列入可自由获取的库名单，因为存在非商业许可证，允许我们免费使用 FMOD。只有当最终应用程序或游戏将商业发行时，才需要付费的 FMOD 许可证。FMOD 支持 Unreal 和 Unity 等 3D 引擎，因此如果你在某个时候从事游戏开发，也可能会接触到 FMOD。

在探索了可以包含哪些用于声音和音乐回放的软件之后，让我们来看看应该播放什么。

## 播放音效

由于声音在我们生活中扮演关键角色，我们对游戏中或模拟中应该听到什么会有一定的期望。未能满足这些期望可能会损害沉浸感。

## 游戏角色的脚步声

玩家最想听到的最重要的音效之一就是角色在地面上的脚步声。通过根据地面材质和角色速度调整声音，可以立即向玩家反馈环境信息。悄悄穿过草地与在金属地面上奔跑之间存在显著差异，玩家应该意识到角色动作的“响亮程度”，并且通过碰撞检测和地面三角形发现，如第 12 章“关卡数据中寻找地面三角形”部分所述，可以轻松找到地面三角形的材质类型。

## 其他角色声音

角色会发出很多声音，不只是脚步声。跳跃和落地、攀爬梯子、游泳或被敌方角色伤害也应为玩家提供听觉反馈，以便了解虚拟世界中角色发生了什么。

## 局部声源

不仅玩家控制的角色需要音效，计算机控制的角色和虚拟世界中的无生命物体也应播放音频效果。听到门打开和关闭、铁匠的敲击声、鸟鸣，以及壁炉的噼啪声、瀑布声或高处世界的风声，将极大地增强沉浸感。

## 环境音

环境音是来自远处位置的不同声音和频率的混合，在传到听者耳中时会失去部分信息。大量人群、森林中的风声或几米外的街道都会产生我们熟知的声音，应添加到虚拟世界的相应位置。

## 天气效果

如果世界包含某种天气系统，也应添加声音和效果。远处的雷暴或下雪后地面的寂静可以改变玩家对虚拟世界的感知。

但音效不仅对沉浸感有用；播放音乐也有助于保持玩家的注意力。

## 播放音乐

自计算机游戏早期以来，音乐就被添加到游戏中以保持玩家的兴趣。因此，我们可能仍然记得看到游戏标题时的音乐，而不记得游戏玩法细节或游戏角色。

## 菜单音乐

玩家启动应用程序时看到的第一个屏幕很可能是一种主菜单。直接将玩家投入行动，而不先让他们配置虚拟角色的控制、视觉质量或声音音量，可能不是最好的主意。在玩家在菜单中探索选项时，音乐可以帮助防止玩家立即迷失或感到无聊。

