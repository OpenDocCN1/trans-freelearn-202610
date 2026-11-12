
![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_428_0.png)

### Figure 12.4 – Project - Description page

- 3. Clicking on Maps & Modes in the left panel will bring you to a page where you can determine which map the game will load by default. Our game has only one map, so that makes this choice easy, but you will often need to designate a map dedicated to your main menu screen to be the first map to load. When you create games with multiple maps, you will need to ensure that the first map loaded can manage which map is loaded next in the play experience.

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_429_0.png)

### Figure 12.5 – Project - Maps & Modes page

- 4. Finally, clicking on the platform you are targeting with this build will bring you to that platform's customization page.

- 5. In the Windows example shown in the following screenshot, the Splash screens and the game's Icon image are available for changing. Mobile and console platform targets will have more options to change, which will be specific to each of those platforms.

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_430_0.png)

### Figure 12.6 – Platforms - Windows page

- 6. Replace the default **SPLASH** and **ICON** settings with the images you would like to use for your game. This can be as simple as an edited screenshot from the game, or you can show off a custom piece of art made specifically for icons and splash screens.

Once you are satisfied with your project settings, close the **Project Settings** window so we can start packaging our project.

## Packaging the game into a build

Creating a distributable form of your game for one of these platforms involves a process called **packaging**. Packaging takes all the code and assets of the game and sets them up in the proper format to be able to perform on the selected platform. We will be following the path to making a Windows or macOS release of your game.

Note

It is important to note that Unreal Engine 5 can only create Windows builds from a copy of the engine running on a Windows system, and OS X builds from copies installed on a macOS running OS X. Thus, the platforms that you can target with your game will be partially limited by the machine you are developing the game on. If you are developing on a Windows PC and wish to create an OS X build of the game, you can install Unreal Engine 5 on a macOS and copy your project files to this new machine. From there, you will be able to generate an OS X build, with no further changes required.

These are the steps to package your game to be played on a particular platform:

- 1. Click the **Platforms** button in the toolbar, hover over the desired platform, and then click on **Package Project**, as follows:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_432_0.png)

### Figure 12.7 – Packaging the project for Windows

- 2. Once you click on a platform, you will be prompted to select a location on your computer to store the build you make. After selecting a location, you will see a popup that tells you that the engine is packaging the project. If something goes wrong in the packaging process, you will be shown the details of the error in the output log window that pops up. Packaging a project can take a bit of time, depending on how complex and large the project is, but if you don't encounter any errors, then you will eventually see a message saying that the packaging is complete. Congratulations – you have created a packaged copy of your game!

- 3. Navigate to the folder where you chose to store your build. On macOS, open the folder called MacNoEditor and double-click on the application to launch it. For Windows, open the WindowsNoEditor folder and double-click on the executable to run the game. Take a moment to go through the game you created in its final form and reflect on just how far you have come. You now have a functional game that other people can play and enjoy. Making even simple games is no easy feat, so you should feel proud of your accomplishment!

We packaged the project using the default settings, but there are a lot of options that you can modify in the packaging.

## Build configurations and packaging settings

In this section, we will see some of the settings available for build and packaging.

The Platforms submenu has the Packaging Settings option, which displays the Project - Packaging page with configuration options that can be modified.

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_434_0.png)

### Figure 12.8 – Project - Packaging page

The **PACKAGING** category contains a lot of technical options that can be used to optimize the packaging. The **Build Configuration** option defines how the build will be done. In Blueprint-only projects, you have two options: **Development** and **Shipping**. Development builds contain information that is used in debugging to help you find errors. Shipping builds are cleaner as they do not have debug information and should be used to create the final version of the game that will be distributed.

The **Staging Directory** property is used to define the folder where the packaged build will be stored. The **For Distribution** option is required when submitting a game to the App Store or Google Play Store.

You can now close the **Project - Packaging** page.

When you want to move the project to another computer or to pass the project to someone else, you can use the **Zip Project** option available in the File menu of the Level Editor. The Zip Project option copies and compresses the essential project files.

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_435_0.png)

### Figure 12.9 – Zip Project

There are many packaging options and some of them are platform-specific. Take the time to study the various packaging options for your game's target platform.

## Summary

In this chapter, we discussed creating playable builds of the game we created across multiple platforms. We learned how to optimize the graphics settings and discussed how to set up the game to be played by others. Finally, we saw how to change the build configurations and packaging settings.

With what you've learned in this chapter, you will be able to better present your project and distribute it on different platforms so that others can enjoy it.

This chapter concludes Part 3 and ends our implementation of the playable shooter game. Part 4 will cover advanced Blueprints concepts. In the next chapter, we will learn about data structures and flow control.

## Quiz

- 1. You can adjust the graphics settings of your game in the **Engine Scalability Settings** panel.

- a. True

- b. False

- 2. In Unreal Engine 5, you can package your game for consoles without being a registered console developer.

- a. True

- b. False

- 3. To package your game, you need to access a menu by clicking the **Platforms** button.

- a. True

- b. False

- 4. Which option is not a **Build Configuration** option in Blueprint-only projects?

- a. Development

- b. Shipping

- c. Test

- 5. The **Staging Directory** property is used to define the folder where the packaged build will be stored.

- a. True

- b. False

## Part 4: Advanced Blueprints

This part will explore advanced Blueprints concepts that will help you when you are developing complex games. We will examine data structures, flow control, math nodes, trace nodes, and Blueprints tips to increase the quality of Blueprints. The final chapter explains some Virtual Reality (VR) concepts and explores the VR template.

This part comprises the following chapters:

- Chapter 13, Data Structures and Flow Control

- Chapter 14, Math and Trace Nodes

- Chapter 15, Blueprints Tips

- Chapter 16, Introduction to VR Development

## Chapter 13: Data Structures and Flow Control

In Part 3, we learned how to create a basic AI game using Behavior Trees, how to add game states, and how to package a game for distribution.

Part 4 will teach you about advanced Blueprint concepts that will help when developing complex games. We will learn about data structures, flow control, math nodes, and Blueprint tips, and gain an understanding of virtual reality development.

This chapter will explain what data structures are and how they can be used to organize data in Blueprints. We will learn about the concept of containers and how to use arrays, sets, and maps to group multiple elements, as well as other ways to organize data using enumerations, structures, and data tables. In this chapter, we will also see how to control a Blueprint's flow of execution by using various types of flow control nodes.

We will cover the following topics in this chapter:

- Exploring different types of containers

- Exploring other data structures

- Flow control nodes

By the end of the chapter, you will know about various data structures and flow control nodes that will increase your ability to solve problems in Blueprints.

### Exploring different types of containers
