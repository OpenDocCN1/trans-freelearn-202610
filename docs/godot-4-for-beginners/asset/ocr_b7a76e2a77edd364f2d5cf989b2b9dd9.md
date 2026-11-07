# Conventions used

There are a number of text conventions used throughout this book.

CodeInText: Indicates code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and X handles. For example: “Now press Ctrl + S to save the scene as player.tscn in our Scenes folder.”

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
var player_name : String = “Riptide”
var player_level : int = 23
var player_exp : float = 123.45
var has_key : bool = false
var spike_damage : int = 10
```

On Linux, run the following command in the terminal:

```
Unzip Godot_v4.3-stable_linux.x86_64.zip -d Godot
```