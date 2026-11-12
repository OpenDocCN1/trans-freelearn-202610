8. Run the sketch in the emulator again by clicking on the **Sketch | Run in Emulator** menu, and then play the game by clicking on the window.

9. With Processing in the **Android** mode, we can run our sketches on hardware devices too. So, if you have a mobile phone or a tablet running on Android, you can connect them to your computer using an USB cable and enable USB debugging in the **Developer** options section of your phone settings. Now you can run the sketch on the device by clicking on the **Sketch | Run on Devices** menu. Here is a picture of my mobile phone running the moon-lander game:

![](bbox=[0.274, 0.294, 0.725, 0.5])

# # Objective Complete - Mini Debriefing

For this task of our current mission, we made our moon-lander game run on Android devices. From step 1 to step 4, we downloaded and installed the Android SDK, which is needed by Processing to compile the Android apps. We then made the game run in an emulator.

In step 6, we tweaked our code a little bit to make the game run in full-screen mode and to fix the device orientation to landscape mode. Since most Android devices come without a keyboard, we also added support for touchscreen input.

Finally, we made our app run on a real device.

# # Classified Intel

The Android mode also allows you to export your sketch as an Android project that can be used to create a signed APK file that can then be used to upload the app to Google Play or if you want to distribute it on your own. You can find detailed instructions on how to do this in the Processing Wiki at `http://wiki.processing.org/w/Android#Distributing_Apps`.