As the last step of level data management, we will add the names of the loaded level files and the per-level settings to the YAML config file.

# Saving and loading the level configuration

Storing the loaded levels and the level settings in the YAM configuration file is quick and straightforward.

After adding an Emitter output operator overload for the LevelSettings data type, we can copy and adjust the emitting code block for models or cameras in the createConfigFile() method of the YamlParser class in the tools folder to save the level data. In addition, we must also emit the selected level number in the settings map of our configuration file.

Also, two new methods named getLevelConfigs() and getSelectedLevelNum() are added to the YamlParser class:

```
std::vector<LevelSettings> getLevelConfigs();
int getSelectedLevelNum();
```

Both methods follow the same process as the model and camera counterparts. The first method, getLevelConfigs(), tries to load the level data from the YAML file, and getSelectedLevelNum() returns the index of the selected level at the time the configuration was saved.

In the YamlParserTypes.h file, a simple pair of encode() and decode() methods for the LevelSettings data type must be added to read back the data from the YAML file. We should also increase the mYamlConfigFileVersion value as the configuration structure has changed.

Now we can add level data from a file, place the level in the virtual world, and store and reload the configuration. Adding the level data to the undo/redo stack is left as an exercise for you, but the basic principle level data undo/redo is identical to the undo/redo functionality for models.

Since the level data may contain overlapping elements, such as bridges, tunnels, or multiple floors of a building, instances could be on the same location in two dimensions. The existing collision detection would be triggered even if the instances are walking at different heights in the level, causing false reactions to non-existing collisions.

We have to extend the quadtree to become an octree to support collision detection in all three dimensions. Let’s check out how to upgrade the quadtree.