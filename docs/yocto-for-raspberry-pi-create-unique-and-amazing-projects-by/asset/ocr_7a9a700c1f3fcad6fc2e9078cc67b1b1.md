Remember to add the device tree needed for the Raspberry Pi 7 touchscreen, as described earlier in the chapter:

```
$ echo "dtoverlay=rpi-ft5406" >> /path/to/sdcard/boot/config.txt
```

Transfer the newly built application to the target and run it:

```
$ /path/to/qt_packpub_app -platform eglfs
```

![](bbox=[0.0, 0.165, 0.36, 0.38])

> *The qt_packpub_app application running on the Raspberry Pi 7 touchscreen*

If you were observant during the earlier steps, you would also have seen that we added a package named qt5-opengles2-test to our image. This package will produce an OpenGL-based Qt application, which we can use to demonstrate multi-touch actions on our touchscreen:

```
$ qt5-opengles2-test -platform eglfs
```