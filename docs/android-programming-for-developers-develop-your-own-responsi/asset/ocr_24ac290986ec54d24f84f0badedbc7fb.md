# Localizing the Where it's snap app

The final and complete code for Where it's snap can be found in the chapter
27/Where Its Snap folder.

Simply add a folder for any languages that you want to add to the app. To add
Spanish, right-click on the res folder and navigate to New | Android resource
directory. Name it values-es. Now, right-click on the folder that we just
created and navigate to New | Android resource file. Name it strings.xml, and
copy and paste the following code into it:

```
<resources>
    <string name="app_name">Donde Es Snap!</string>

    <string name="action_settings">Ajustes</string>
    <string name="drawer_open">Abra</string>
    <string name="drawer_close">Cerca</string>

    <string name="enter_photo_title">Ingrese título de la
foto</string>
    <string name="tag1">Tag 1</string>
    <string name="tag2">Tag 2</string>
    <string name="tag3">Tag 3</string>
    <string name="capture">Tomar foto</string>
    <string name="save">Guardar</string>

    <string name="tags">Tags</string>
    <string name="titles">Títulos</string>
    <string name="show_map">Ver mapa</string>

    <!-- These are the items in our list -->
    <string-array name="nav_drawer_items">
        <item >Títulos</item>
        <item >Tags</item>
        <item >Tomar foto</item>
    </string-array>
</resources>
```

You can now switch the device to Spanish, as we did for the last mini app, and
run Where it's snap to see it as it would look if somebody in Spain downloaded it
from the app store. This next screenshot shows you CaptureFragment in action: