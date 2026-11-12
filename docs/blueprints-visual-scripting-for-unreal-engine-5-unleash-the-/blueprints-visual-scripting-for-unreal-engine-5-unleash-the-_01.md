# Preface

Unreal Engine's Blueprint visual scripting system enables designers to script their games as well as programmers to create base elements that can be extended by designers. With this book, you'll explore all the features of the Blueprint Editor, along with expert tips, shortcuts, and best practices.

The book guides you through using variables, macros, and functions, and helps you learn about object-oriented programming (OOP). You'll discover the Gameplay Framework and advance to learning how Blueprint Communication allows one Blueprint to access information from another Blueprint. Later chapters will focus on building a fully functional game step by step. You'll start with a basic first-person shooter (FPS) template, and each chapter will build on the prototype to create an increasingly complex and robust game experience. You'll then progress from creating basic shooting mechanics to more complex systems, such as user interface elements and intelligent enemy behavior. The book demonstrates how to use arrays, maps, enums, and vector operations and introduces the elements needed for VR game development. In the final chapters, you'll learn how to implement procedural generation and create a product configurator.

By the end of this book, you'll have learned how to build a fully functional game and have the skills required to develop an entertaining experience for your audience.

# Who this book is for

This book is for anyone interested in developing games or applications with Unreal Engine 5. Whether you are brand new to game development or have just not had any exposure to Unreal Engine 5's Blueprint Visual Scripting system, this is a great place to start learning about how to build complex game mechanics quickly and easily without writing any text code. No programming experience is required!

# What this book covers

- *Chapter 1, Exploring the Blueprint Editor,* covers the Blueprint Editor and all the panels that are integrated into it. We will explore the **Components** tab, the **My Blueprint** tab, the **Details** tab, and the **Viewport** and **Event Graph** tabs. Then, we will go through what components are and how to add them to a Blueprint.
- *Chapter 2, Programming with Blueprints,* explains programming concepts that are used in Blueprints. We will learn about how to use variables, operators, events, actions, macros, and functions.
- *Chapter 3, Object-Oriented Programming and the Gameplay Framework,* teaches OOP concepts and explores the Gameplay Framework.
- *Chapter 4, Understanding Blueprint Communication,* explores different types of Blueprint Communication, which allows one Blueprint to access the information of another Blueprint.
- *Chapter 5, Object Interaction with Blueprints,* covers how to bring new objects to a level to help build the world in which the game will be set. We will move on to manipulating materials on objects, first through the object editor, and then by triggering during runtime via Blueprints.
- *Chapter 6, Enhancing Player Abilities,* teaches you how to use Blueprints to generate new objects during gameplay, and how to link actions in Blueprints to player control inputs. You'll also learn about how to create Blueprints that allow objects to react to collisions with our generated projectiles.
- *Chapter 7, Creating Screen UI Elements,* demonstrates setting up a **graphical user interface (GUI)** that will track the player's health, stamina, ammo, and current objective. Here, you will learn how to set up a basic user interface using Unreal's GUI Editor and how to use Blueprints to link the interface to the gameplay values.
- Chapter 8, Creating Constraints and Gameplay Objectives, covers how to constrain the player's abilities, define the gameplay objectives for a level, and track those objectives. We'll walk through setting up collectible ammo packs that will refill the ammo of the player's gun, as well as utilizing the level Blueprint to define a win condition for our game.
- Chapter 9, Building Smart Enemies with Artificial Intelligence, is a crucial chapter that covers how to create an enemy zombie AI that will pursue the player around the level. We'll walk through setting up a navigation mesh on our level and see how to use Blueprints to get enemies to traverse between patrol points.
- Chapter 10, Upgrading the AI Enemies, teaches us how to create a compelling experience by modifying the zombie AI to have states in order to give the zombies a little more intelligence. In this chapter, we'll set up the patrol, searching, and attack states for the zombies by using visual and auditory detection. Additionally, we'll explore how to make new enemies appear gradually as the game is playing.
- Chapter 11, Game States and Applying the Finishing Touches, adds the finishing touches that are required to make our game a complete experience before we finalize our game for release. In this chapter, we'll create rounds that will make the game increasingly difficult, game saves so that the player can save their progress and return, and player death to make the game's challenge meaningful.
- Chapter 12, Building and Publishing, covers how to optimize graphics settings to get our game performing and looking its best, and how to set up project information for distribution. Then, we'll learn about how to create shareable builds of the game for various platforms.
- Chapter 13, Data Structures and Flow Control, explains what data structures are and how they can be used to organize data in Blueprints. We'll learn about the concept of containers and how to use arrays, sets, and maps to group multiple elements. This chapter shows other ways to organize data using enumerations, structures, and data tables. In this chapter, we'll also see how to control the flow of execution of a Blueprint by using various types of flow control nodes.
- Chapter 14, Math and Trace Nodes, covers some math concepts that are required for 3D games. We will learn the difference between world and local coordinates and how to use them when working with components. This chapter shows us how to use vectors to represent the position, direction, velocity, and distance. The concept of traces is explained and various types of traces are presented. We'll also see how to use traces to test collisions in the game.
- Chapter 15, Blueprints Tips, contains several tips to increase the quality of Blueprints. We will learn about how to use various editor shortcuts that speed up our work. This chapter demonstrates some Blueprint best practices that will help you decide where and what types of implementation should be undertaken. Finally, we'll learn about more useful Blueprint miscellaneous nodes.
- Chapter 16, Introduction to VR Development, explains some VR concepts and explores the VR template. This chapter explores the functionalities of the VRPwn Blueprint of the VR template and explains how to create objects that can be grabbed by the player using motion controllers. We will learn about the Blueprint functions used to implement teleportation and how to use the interface for Blueprint communication. We will also see how the menu works in the VR template.
- Chapter 17, Animation Blueprints, presents the main elements of the Unreal Engine animation system, including Skeleton, Skeletal Mesh, Animation Sequences, and Blend Spaces. It shows how to script an Animation Blueprint using the Event Graph and the Anim Graph. It explains how state machines are used in an animation and how to create new states for an animation.
- Chapter 18, Creating Blueprint Libraries and Components, shows how to create Blueprint Macro and Function Libraries with common functionalities that can be used throughout the project. It explains in more detail the concept of components. We will also learn how to create Actor components with encapsulated behavior and Scene components with location-based behavior.





第 19 章，程序化生成，展示了几种自动生成关卡内容的方法。您可以使用蓝图的构造脚本来编写程序化生成，并使用样条线工具定义一条路径，该路径将用作定位实例的参考。此外，您可以创建编辑器实用工具蓝图来在编辑模式下操作资产和角色。

第 20 章，使用变体管理器创建产品配置器，解释了如何创建产品配置器，这是一种用于在行业中吸引消费者关注特定产品的应用程序类型。您将学习如何使用变体管理器面板和变体集来定义产品配置器。产品配置器模板是实践中研究各种蓝图概念的绝佳资源。我们将分析 BP_Configurator 蓝图，它使用带有变体集的 UMG 小部件蓝图动态创建用户界面。

附录，测验答案，包含所有测验问题的答案，按章节排列。

# 充分利用本书

虽然需要一些 Windows 操作系统或 macOS 的基础知识，但不需要编程或虚幻引擎 5 的经验。

本书专注于虚幻引擎 5，这意味着您只需要一份虚幻引擎即可开始。虚幻引擎 5 可以从 https://www.unrealengine.com/ 免费下载，并附带您跟随本书所需的一切。

# 下载示例代码文件

本书的代码包也托管在 GitHub 上，网址为 https://github.com/PacktPublishing/-Blueprints-Visual-Scripting-for-Unreal-Engine-5。如果代码有更新，将在现有的 GitHub 仓库中更新。

我们还有来自丰富书籍和视频目录的其他代码包，可在 https://github.com/PacktPublishing/ 上查看。请查看！

# 下载彩色图片

我们还提供一个 PDF 文件，其中包含本书中使用的屏幕截图和图表的彩色图片。您可以在此处下载：https://static.packt-cdn.com/downloads/9781801811583_ColorImages.pdf。

# 本书使用的惯例

本书中使用了若干文本惯例。

`Code in text`：表示文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 帐号。示例："在关卡编辑器中，选择我们放置在关卡上的 `BP_EnemyCharacter` 实例。"

**粗体**：表示新术语、重要单词或屏幕上看到的单词。例如，菜单或对话框中的单词以 **粗体** 显示。示例："将 **参数名称** 更改为 **金属**，然后单击并拖动我们 **金属** 节点的输出引脚到 **材质定义** 节点的 **金属** 输入引脚。"

提示或重要说明

显示如下。

# 联系我们

我们始终欢迎读者反馈。

**一般反馈**：如果您对本书的任何方面有疑问，请发送电子邮件至 customercare@packtpub.com，并在邮件主题中注明书名。

**勘误**：尽管我们已尽一切努力确保内容的准确性，但错误难免发生。如果您在本书中发现错误，我们将不胜感激如果您能向我们报告。请访问 www.packtpub.com/support/errata 并填写表格。

**盗版**：如果您在互联网上以任何形式发现我们作品的非法副本，我们将不胜感激如果您能向我们提供位置地址或网站名称。请带上材料链接联系我们，邮箱为 copyright@packt.com。

**如果您有兴趣成为作者**：如果您在某个主题上有专业知识，并有兴趣撰写或参与编写图书，请访问 authors.packtpub.com。

# 分享您的想法

读完《蓝图可视化脚本用于虚幻引擎 5》后，我们很乐意听取您的想法！请单击此处直接前往本书的 Amazon 评论页面并分享您的反馈。

您的评论对我们和技术社区很重要，并将帮助我们确保提供高质量的内容。
