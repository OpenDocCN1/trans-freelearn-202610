# Visualizing the script progress

Especially when executing complex scripts that take a while to finish, it is good practice to display the progress of the script execution in a progress bar. To add a progress bar to the previous script, we can add the following lines of code before and inside the for loop that loops through the input features:

```
i = 0
n = my_layer.featureCount()
for input_feature in my_layer.getFeatures():
    progress.setPercentage(int(100*i/n))
    i+=1
```

> Note that we initialize the i counter before the loop and increase it inside the loop after updating the progress bar using progress.setPercentage().

# Developing your first plugin

When you want to implement interactive tools or very specific graphical user interfaces, it is time to look into plugin development. In the previous exercises, we introduced the QGIS Python API. Therefore, we can now focus on the necessary steps to get our first QGIS plugin started. The great thing about creating plugins for QGIS is that there is a plugin for this! It's called Plugin Builder. And while you are at it, also install Plugin Reloader, which is very useful for plugin developers. Because it lets you quickly reload your plugin without having to restart QGIS every time you make changes to the code. When you have installed both plugins, your Plugins toolbar will look like this:

![](bbox=[0.315, 0.6, 0.68, 0.68])

Before we can get started, we also need to install Qt Designer, which is the application we will use to design the user interface. If you are using Windows, I recommend WinPython (http://winpython.github.io/) version 2.7.10.3 (the latest version with Python 2.7 at the time of writing this book), which provides Qt Designer and Spyder (an integrated development environment for Python). On Ubuntu, you can install Qt Designer using sudo apt-get install qt4-designer. On Mac, you can get the Qt Creator installer (which includes Qt Designer) from http://qt-project.org/downloads.