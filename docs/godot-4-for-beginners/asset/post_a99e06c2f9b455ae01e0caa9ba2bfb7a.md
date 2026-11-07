
## Conventions used

There are a number of text conventions used throughout this book.

CodeInText: Indicates code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and X handles. For example: "Now press Ctrl + S to save the scene as player.tscn in our Scenes folder."

A block of code is set as follows:

```
extends Node2D

@onready var update_health_hud = $HUD
@onready var player = $Player
@onready var strawberries = $Collectibles

func _ready():
    player.level_strawberries = strawberries.get_child_count()

func _process(delta):
    update_health_hud.frame = player.get_hearts()
```

When we wish to draw your attention to a command-line input or output in a set of steps, the relevant lines or items are set in bold:

```
extends Node2D
var health : int = 100
var jump_strength : float = 220.5
var is_dashing : bool = true
var player_name : String = "Riptide"
var player_level : int = 23
var player_exp : float = 123.45
var has_key : bool = false
var spike_damage : int = 10
```

On Linux, run the following command in the terminal:

```
Unzip Godot_v4.3-stable_linux.x86_64.zip -d Godot
```

**Bold:** Indicates a new term, an important word, or words that you see on the screen. For instance, words in menus or dialog boxes appear in text like this. For example: "Enable **Rotate Y** and **Disable Z** in the **Particle Flags** settings."

Warnings or important notes appear like this.

Tips and tricks appear like this.

## Get in touch

Feedback from our readers is always welcome.

**General feedback:** If you have questions about any aspect of this book or have any general feedback, please email us at customercare@packt.com and mention the book’s title in the subject of your message.

**Errata:** Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you have found a mistake in this book, we would be grateful if you reported this to us. Please visit http://www.packt.com/submit-errata, click **Submit Errata**, and fill in the form.

**Piracy:** If you come across any illegal copies of our works in any form on the internet, we would be grateful if you would provide us with the location address or website name. Please contact us at copyright@packt.com with a link to the material.

**If you are interested in becoming an author:** If there is a topic that you have expertise in and you are interested in either writing or contributing to a book, please visit http://authors.packt.com/.

## Share your thoughts

Once you’ve read *Godot 4 for Beginners*, we’d love to hear your thoughts! Please click here to go straight to the Amazon review page for this book and share your feedback.

Your review is important to us and the tech community and will help us make sure we’re delivering excellent quality content.

## Join our community on Discord

Join our community’s Discord space for discussions with the author and other readers:

```
https://packt.link/godot-4-game-dev
```

![](img/d34207e07c133a9e84d9caf1d479006c_23_0.png)

## Part 1

### Learning How the Godot Engine Works

In this first part of the book, you’ll build a strong foundation in using the Godot game engine. We’ll begin by exploring Godot’s origins and what makes it a unique and powerful tool for game development. Then, you’ll get hands-on with the engine’s interface, learning how scenes, nodes, and the editor all work together. You’ll also take your first steps into 3D game development and dive into scripting with GDScript, Godot’s intuitive, Python-like language. By the end of this part, you’ll be ready to begin creating interactive games with confidence.

This part of the book includes the following chapters:

- *Chapter 1, Let’s Get Godot-ing!*
- *Chapter 2, Exploring the Godot Engine Interface*
- *Chapter 3, Introduction to 3D*
- *Chapter 4, Scripting with GDScript*

## 1

### Let’s Get Godot-ing!

In this chapter, we’ll explore the history and core features of Godot 4.0, a game development engine renowned for its versatility and accessibility. Whether you’re an aspiring indie developer or a seasoned creator looking for a new tool, understanding what makes Godot unique will set the foundation for your journey into game development.

This chapter serves two main purposes. The first is to introduce you to Godot’s background and key capabilities, showing why it has become a popular choice in the game development community. Next is to guide you through setting up Godot 4.0 and creating your first project, ensuring you have the tools and confidence to begin crafting your own games.

In this chapter, we’re going to cover the following main topics:

- What is Godot?
- Setting up Godot 4
- Creating a new project

By the end of this chapter, you’ll not only have grasped the essential features that make Godot stand out, but also have the knowledge to install, launch, and create your first project in the engine. These are fundamental steps that lay the groundwork for everything we’ll accomplish in later chapters.

## Getting the most out of this book — get to know your free benefits

Unlock exclusive free benefits that come with your purchase, thoughtfully crafted to supercharge your learning journey and help you learn without limits.

Here’s a quick overview of what you get with this book:

### Next-gen reader

![](img/d34207e07c133a9e84d9caf1d479006c_28_0.png)

Figure 1.1: Illustration of the next-gen Packt Reader's features

Our web-based reader, designed to help you learn effectively, comes with the following features:

- Multi-device progress sync: Learn from any device with seamless progress sync.
- Highlighting and notetaking: Turn your reading into lasting knowledge.
- Bookmarking: Revisit your most important learnings anytime.
- Dark mode: Focus with minimal eye strain by switching to dark or sepia mode.

### Interactive AI assistant (beta)

![](img/d34207e07c133a9e84d9caf1d479006c_29_0.png)

Figure 1.2: Illustration of Packt's AI assistant

Our interactive AI assistant has been trained on the content of this book, to maximize your learning experience. It comes with the following features:

- ✨ Summarize it: Summarize key sections or an entire chapter.
- ✨ AI code explainers: In the next-gen Packt Reader, click the Explain button above each code block for AI-powered code explanations.

Note: The AI assistant is part of next-gen Packt Reader and is still in beta.

### DRM-free PDF or ePub version

![](img/d34207e07c133a9e84d9caf1d479006c_29_1.png)

Figure 1.3: Free PDF and ePub

Learn without limits with the following perks included with your purchase:

- 📄 Learn from anywhere with a DRM-free PDF copy of this book.
- 📄 Use your favorite e-reader to learn using a DRM-free ePub version of this book.

## Unlock this book’s exclusive benefits now

Scan this QR code or go to packtpub.com/unlock, then search for this book by name. Ensure it’s the correct edition.

Note: Keep your purchase invoice ready before you start.

![](img/d34207e07c133a9e84d9caf1d479006c_30_0.png)

![](img/d34207e07c133a9e84d9caf1d479006c_30_1.png)

## Technical requirements

You can find the example project and code for this book in the GitHub repository: https://github.com/PacktPublishing/Godot-4-for-Beginners

This chapter’s code files are available here: https://github.com/PacktPublishing/Godot-4-for-Beginners/tree/main/ch1/godotintro

Visit this link to check out the video of the code being run: https://packt.link/RjbW3

### What is Godot?

The short answer is that Godot is a game engine. It was originally created in 2007 by Juan Linietsky and Ariel Manzur as an in-house tool for various Argentinian studios. Your next questions might be, “What exactly is a game engine?” and “What makes Godot so special?”

Well, making a game is hard—really hard! There are countless moving parts that all need to work together perfectly. While it’s possible to build everything from scratch, most developers rely on a game engine to handle the heavy lifting.

So, a game engine is a software framework that simplifies game development. It provides the core tools and systems for building a game—such as rendering, physics, input handling, and asset management—so that developers can focus more on gameplay, story, and design, rather than low-level technical code.
