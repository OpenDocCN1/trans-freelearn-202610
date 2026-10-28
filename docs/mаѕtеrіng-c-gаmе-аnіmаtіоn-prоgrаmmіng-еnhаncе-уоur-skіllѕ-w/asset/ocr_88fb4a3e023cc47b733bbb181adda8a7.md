```
mYamlEmit << YAML::EndSeq;
 mYamlEmit << YAML::EndMap;
```

In the saved YAML configuration file, a new sequence will appear for each camera, storing all camera settings as key/value pairs:

```
cameras:
  - camera-name: FreeCam
    position: [2.9061296, 11.1587305, 64.1114578]
    view-azimuth: 289.300262
    view-elevation: -34.4999695
```

Since we made essential changes to the configuration file, we should reflect the new on-disk format with a new version number.

# Bumping the configuration file version

Adjusting the configuration file version number in the emitter is easy; we just have to raise the version number string from 1.0 to something like 1.1 or 2.0. From now on, all saved configuration files will have the new version number.

Loading a configuration file gets a bit more complex now. If we want to support the old file format, we must retain all methods that were used to parse the previous file version contents. After reading the version number of the file to parse, we must decide if we want to parse the new sequence style for multiple cameras, or get the settings for the single camera and apply those settings to the always-available default free camera. Saving the configuration could be done in the new version, resulting in a migration of a configuration file from the old to the new version.

Updating the configuration by supporting the reading of an older version is one of the tasks in the Practical sessions section.

We can create a bunch of free-floating cameras now, position the cameras in our virtual world, and switch between the cameras via the user interface. Still sounds a bit boring, doesn’t it? Let’s enhance the application more by adding different camera types.

# Creating different camera types

Having the ability to add more than one camera in the virtual world is a good basis for adding a couple of predefined camera settings. The different camera types behave in different ways regarding the movement of the view and the movement of the camera itself.