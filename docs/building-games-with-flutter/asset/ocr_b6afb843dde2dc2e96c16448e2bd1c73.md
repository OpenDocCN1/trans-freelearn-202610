```
color: Colors.red, fontSize: 32.0))),
                ),
            )),
        );
    }
```

Here, we have a simple widget that draws the menu, which is a simple column of menu items in the middle, with a title at the top and an exit option below the menu items. Clicking on these options tells the navigator to change to a different page.

Here, you can see how the menu screen should look:

![](bbox=[0.25, 0.35, 0.745, 0.585])

Figure 11.1 – The game menu screen, allowing you to play the game or view the settings

Next, let's continue for now with the settings screen.

# ## Adding a settings screen

In this section, we will add a settings screen to allow us to choose the volume of the background music by following these steps:

+   1. Open the `screen_settings.dart` file and add the code from here: https://github.com/PacktPublishing/Building-Games-with-Flutter/blob/main/chapter11/lib/widgets/screen_settings.dart.