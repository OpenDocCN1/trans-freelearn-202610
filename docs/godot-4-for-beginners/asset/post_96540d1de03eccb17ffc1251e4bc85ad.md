
## What makes Godot so special?

There are many reasons why Godot has become a popular choice for both beginner and experienced game developers. Here are some of the core features that make this engine stand out from the rest:

- **Open source and free:** In 2014, Godot was released as open source under the MIT license. That means it’s completely free to use—even for commercial projects—and developers have full access to the source code. This makes Godot highly customizable and community-driven.
- **Versatile scene system:** One of Godot’s most defining features is its scene system. Everything in Godot is a scene—and scenes are made up of nodes. Scenes can also be nested inside other scenes, making it easy to build complex, reusable, and modular components. This structure makes organizing and scaling a game project incredibly intuitive.
- **Powerful 2D and 3D support:** Godot includes dedicated tools for both 2D and 3D development. Whether you’re making a pixel-art platformer or a fully-fledged 3D experience, Godot provides specialized workflows for both dimensions. While other engines also support both, Godot’s 2D system in particular is often praised for its precision and flexibility.
- **Resources—the other side of the coin:** Alongside scenes and nodes, Godot uses something called resources—and they’re everywhere. A texture on a sprite is a resource. The audio you play is a resource. Even the path in a Path2D node is a resource. Resources are data containers that can be reused across your project, edited in the inspector, or saved and loaded from disk. They promote consistency, reduce duplication, and keep your projects lightweight and organized.
- **Lightweight and cross-platform:** Godot is lightweight and quick to install and runs smoothly even on older machines. This low system requirement is a huge win for accessibility and inclusion—anyone can start making games without needing a high-end PC. Plus, when your game is finished, you can export it to a wide variety of platforms, including Windows, macOS, Linux, Android, iOS, and HTML5.
- **Thriving community and active development:** Godot has a friendly, growing community of developers and educators. Whether you’re just starting out or are an experienced coder, there’s plenty of support through forums, Discord channels, tutorials, and documentation. The engine is under continuous development with regular updates and an open roadmap.

Now that you’ve learned about the key features that make Godot such a powerful and beginner-friendly game engine—from its flexible node and scene system to its vibrant open-source community—you have a strong understanding of why it is a great choice for your game development journey.

It’s time to move from theory to practice!

In the next section, we’ll guide you through downloading and installing Godot, and help you run the engine for the very first time. Soon, you’ll be setting up your own projects and building your first games step by step. Let’s get started!

## Setting up Godot 4

To download the latest version of Godot 4, follow these steps:

1. Go to Godot’s website (https://www.godotengine.org) and click on the **Download Latest** button, shown in Figure 1.4. Note that version 4.3 was the version in use at the time of writing (see *Preface* for details).

![](img/d34207e07c133a9e84d9caf1d479006c_32_0.png)

*Figure 1.4 – The download page for Godot Engine 4 for Windows*

2. The website should detect your operating system platform and take you to the correct download. Click on the **Godot Engine** button rather than the .NET one:

![](img/d34207e07c133a9e84d9caf1d479006c_33_0.png)

Figure 1.5 – The browser has detected the Windows operating system

3. If your platform cannot be detected, you can choose from one of the supported platforms by scrolling further down the page:

![](img/d34207e07c133a9e84d9caf1d479006c_33_1.png)

Figure 1.6 – Choose your own platform from the available options

4. Once the download is complete, it’s time to create our folder structure.

5. On Windows, open the ZIP file. The main executable will be named something such as Godot_v4.3-stable_win64.exe.

- On macOS, double-click the ZIP file
- On Linux, run the following command in the terminal:

```
Unzip Godot_v4.3-stable_linux.x86_64.zip -d Godot
```

6. Extract this file to a folder on your PC. I recommend naming the folder Godot4.

7. Within this folder, create another folder called Godot 4 Projects. This is the folder where you will store all your projects made with Godot 4:

![](img/d34207e07c133a9e84d9caf1d479006c_34_0.png)

Figure 1.7 – Godot folder structure

8. Time to launch Godot! *Right-click* on the .exe file and choose **Pin to Start** to have Godot available in the **Start** menu:

![](img/d34207e07c133a9e84d9caf1d479006c_34_1.png)

Figure 1.8 – How to pin to Start

9. You can also click on Show more options and then choose Pin to taskbar so that Godot is always ready and waiting at the bottom of your screen:

![](img/d34207e07c133a9e84d9caf1d479006c_35_0.png)

Figure 1.9 – Show more options and Pin to taskbar

10. Double-click on the extracted .exe file within the Godot 4 folder to launch Godot 4!

Godot is up and running. Our next step is to create our first project—our wonderful adventure in game development starts now!

## Creating a new project

Having followed the steps in the previous section, you are now presented with the Godot Project Manager:

![](img/d34207e07c133a9e84d9caf1d479006c_36_0.png)

Figure 1.10 – The Godot Project Manager

This is always the first window you see when you launch Godot. Here, you can create, remove, import, edit, and play game projects.

Because of all the black ink needed to print images in dark mode, we will do our bit to save the planet by switching to light mode. There is no difference other than appearance. To do that, follow these steps:

1. Click on the Settings button in the top-right corner:

![](img/d34207e07c133a9e84d9caf1d479006c_37_0.png)

Figure 1.11 – The Project Manager Settings button

2. Now, change the Interface Theme dropdown to Light:

![](img/d34207e07c133a9e84d9caf1d479006c_37_1.png)

Figure 1.12 – Changing to the Light theme in the Quick Settings menu

Now, let’s walk through the steps to set up a new project in Godot 4:

1. Click on the + Create button in the top-left corner to create a new project:

![](img/d34207e07c133a9e84d9caf1d479006c_38_0.png)

*Figure 1.13 – How to create a new project in the Godot Project Manager*

2. We are now presented with a new window in which we can adjust the details of the project:

![](img/d34207e07c133a9e84d9caf1d479006c_38_1.png)

*Figure 1.14 – The Create New Project sub-menu*

3. The next step is to give our project a name. Let’s call it GodotIntro.

Also, ensure that Project Path points to the Godot 4 Projects folder we created earlier and that the Create Folder button is toggled on:

![](img/d34207e07c133a9e84d9caf1d479006c_39_0.png)

Figure 1.15 – Initial settings for our new Godot project

4. This will create a new folder within Godot 4 Projects named GodotIntro. Creating a new folder for each new project in Godot will help us keep everything organized.

You can leave Renderer in Compatibility mode. Each renderer option is explained here:

- **Forward+** is a renderer that is aimed at improving the rendering of lights on desktop platforms. This is not recommended for mobile development as it is not efficient.
- **Mobile** uses a more efficient approach for lighting that is intended for mobile platforms, but can also run on a desktop. However, this renderer does not provide high-end graphical features.
- **Compatibility** has even fewer high-end graphical features than **Mobile**, but it will run on most hardware.

5. Click on **Create & Edit**, and you will be presented with the following window:

![](img/d34207e07c133a9e84d9caf1d479006c_40_0.png)

Figure 1.16 – The main Godot engine editor interface

6. By default, Godot opens in 3D view. We can change this using the view menu at the top; however, Godot will change the view automatically once we add our first node.

![](img/d34207e07c133a9e84d9caf1d479006c_40_1.png)

Figure 1.17 – The context menu to switch views

From here, we will create our first little project and then take a closer look at the interface in the next chapter:

1. Click on **2D Scene** in the top left of the screen:

![](img/d34207e07c133a9e84d9caf1d479006c_40_2.png)

Figure 1.18 – The Scene window
