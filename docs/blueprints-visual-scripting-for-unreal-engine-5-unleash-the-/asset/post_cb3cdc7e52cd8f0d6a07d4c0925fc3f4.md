
Chapter 19, Procedural Generation, shows several ways to generate level content automatically. You can use the construction script of a Blueprint to script procedural generation and use the Spline tool to define a path that will be used as a reference to position the instances. Also, you can create an Editor Utility Blueprint to manipulate assets and actors in edit mode.

Chapter 20, Creating a Product Configurator Using the Variant Manager, explains how to create a product configurator, which is a type of application used in industry to attract consumers to a specific product. You will learn how to use the Variant Manager panel and variant sets to define a product configurator. The Product Configurator template is an excellent resource for studying various Blueprint concepts in practice. We will analyze the BP_Configurator Blueprint, which dynamically creates the user interface using UMG widget Blueprints with the variant sets.

Appendix, Quiz answers, contains the answers to all the quiz questions, chapter-wise.

## To get the most out of this book

Although some basic knowledge of the Windows OS or macOS is required, experience in programming or Unreal Engine 5 is not necessary.

This book is focused on Unreal Engine 5, which means you only need a copy of Unreal Engine to get started. Unreal Engine 5 can be downloaded for free from https://www.unrealengine.com/ and comes with everything you need to follow along with this book.

## Download the example code files

The code bundle for the book is also hosted on GitHub at https://github.com/PacktPublishing/-Blueprints-Visual-Scripting-for-Unreal-Engine-5. If there's an update to the code, it will be updated on the existing GitHub repository.

We also have other code bundles from our rich catalog of books and videos available at https://github.com/PacktPublishing/. Check them out!

## Download the color images

We also provide a PDF file that has color images of the screenshots and diagrams used in this book. You can download it here: https://static.packt-cdn.com/downloads/9781801811583_ColorImages.pdf.

## Conventions used

There are a number of text conventions used throughout this book.

`Code in text`: Indicates code words in the text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles. Here is an example: "In the Level Editor, select the instance of `BP_EnemyCharacter` that we placed on the level."

**Bold**: Indicates a new term, an important word, or words that you see on screen. For instance, words in menus or dialog boxes appear in **bold**. Here is an example: "Change **Parameter Name** to **Metallic**, and then click and drag the output pin from our **Metallic** node to the **Metallic** input pin of the **Material definition** node."

Tips or Important Notes

Appear like this.

## Get in touch

Feedback from our readers is always welcome.

**General feedback**: If you have questions about any aspect of this book, email us at customercare@packtpub.com and mention the book title in the subject of your message.

**Errata**: Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you have found a mistake in this book, we would be grateful if you would report this to us. Please visit www.packtpub.com/support/errata and fill in the form.

**Piracy**: If you come across any illegal copies of our works in any form on the internet, we would be grateful if you would provide us with the location address or website name. Please contact us at copyright@packt.com with a link to the material.

**If you are interested in becoming an author**: If there is a topic that you have expertise in and you are interested in either writing or contributing to a book, please visit authors.packtpub.com.

## Share Your Thoughts

Once you've read Blueprints Visual Scripting for Unreal Engine 5, we'd love to hear your thoughts! Please click here to go straight to the Amazon review page for this book and share your feedback.

Your review is important to us and the tech community and will help us make sure we're delivering excellent quality content.

## Part 1: Blueprint Fundamentals

This part will explore the basic building blocks of Blueprints. You will gain a solid understanding of how Blueprints work and will be able to start creating your own games.

This part comprises the following chapters:

- Chapter 1, Exploring the Blueprint Editor
- Chapter 2, Programming with Blueprints
- Chapter 3, Object-Oriented Programming and the Gameplay Framework
- Chapter 4, Understanding Blueprint Communication

## Chapter 1: Exploring the Blueprint Editor

Welcome to the amazing world of game development with Unreal Engine 5. In this book, we will learn how to develop games in Unreal Engine using the Blueprints Visual Scripting language, which was created by Epic Games for Unreal Engine.

The first step that is needed before we can learn about Blueprints is to prepare our development environment. Unreal Engine is free to download. We will learn how to install Unreal Engine 5 and create a new project. After that, we will learn about some of the basic concepts of Blueprints and explore each panel of the Blueprint Editor.

In this chapter, we will cover the following topics:

- Installing Unreal Engine
- Creating new projects and using templates
- Blueprints Visual Scripting
- The Blueprint Class Editor interface
- Adding Components to a Blueprint

### Installing Unreal Engine

To use Unreal Engine, you must first install the Epic Games Launcher:

1. Access the website at https://www.unrealengine.com.
2. Register and download the Epic Games Launcher.
3. Install and start the launcher.
4. Click the Unreal Engine tab on the left-hand side.
5. Click on the **Library** tab that appears at the top of the screen.
6. Click the + button next to **ENGINE VERSIONS** to add a version of Unreal Engine to the launcher. You can use the newest version available.
7. Click the **Install** button. The launcher will start downloading the files needed for installation and may take a long time to complete.
8. Click the **Launch** button to start an already installed version. It is possible to have multiple versions of Unreal Engine installed on the same machine – you simply set one of them as the current version. The **Launch** button at the top right of the launcher will start the current version.

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_14_0.png)

Figure 1.1 – Launching Unreal Engine

The Blueprint Visual Scripting system is already a well established and stable technology. This book uses version 5.0.0, but the examples created in this book should work without problems in later versions.

### Creating new projects and using templates

After starting up Unreal Engine Editor, the **Unreal Project Browser** will appear. The **Recent Projects** box on the top left is used to open existing projects and the other boxes on the left are categories of templates used to create a new project. The following screenshot shows the templates of the Games category.

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_15_0.png)

Figure 1.2 – Templates under the Games category

Templates are basic containers with some key files and a level that provide a basic starting point for different types of projects. They are useful for rapid prototyping or to learn the basic mechanics of a specific type of project. The use of templates is optional. All templates can be simply recreated in the blank template. The following are descriptions of each template in the Games category:

- **First Person**: For games with a first-person perspective. This template features a player character represented by a pair of arms equipped with a gun that fires a simple sphere projectile. The character can be moved around the level using a keyboard, controller, or virtual joystick on a touch device.
- **Handheld AR**: For augmented reality applications for Android and iOS devices. This template features runtime logic for toggling AR mode
