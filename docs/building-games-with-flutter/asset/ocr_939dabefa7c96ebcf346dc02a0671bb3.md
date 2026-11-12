![](bbox=[0.303, 0.092, 0.684, 0.283])

Figure 11.3 – The game over screen when George dies

Now that we have our three extra screens, let's tie these together with navigation routes and music volume.

# Compiling all screens with navigation routes and music volume

In this section, we will tie all the navigation together and also adjust the music volume, based on the settings menu choice. To do this, we will follow these steps:

+   1. Open the main.dart file and import the screens and shared preference library:

```
import
'package:goldrush/widgets/screen_gameover.dart';
import 'package:goldrush/widgets/screen_menu.dart';
import
'package:goldrush/widgets/screen_settings.dart';
import
'package:shared_preferences/shared_preferences.dart';
```

+   2. Let's rewrite the main function to use routes. Replace the existing main function with the following:

```
void main() async {
WidgetsFlutterBinding.ensureInitialized();
```