We will keep this simple for now, but it's worth keeping JoystickComponent in its own file in case you want to style it more later, by using custom images for the joystick instead of colors.

+   13. Now that we have our joystick, run button, and score text components, let's create a HUD component that will handle the initialization of these components. First, open the hud.dart file and add the following imports and class definition:

```
import 'package:flame/components.dart';
import
'package:goldrush/components/hud/run_button.dart';
import
'package:goldrush/components/hud/score_text.dart';
import
'package:goldrush/components/hud/joystick.dart';
import 'package:flame/palette.dart';
import 'package:flutter/material.dart';
class HudComponent extends PositionComponent {
}
```

Here, we extend from PositionComponent, giving us control over how we arrange our child components with margins. PositionComponent is a base class that allows the positioning of components within it.

+   14. At the top of the HudComponent class, let's add a constructor that sets priority to 20:

```
HudComponent() : super(priority: 20);
```

priority is used to indicate the order components are drawn, with the default being 0. So, setting this higher than 0 will ensure that the HUD is drawn on top of everything else.

Later, when we start using a map that is larger than the physical screen, the character and enemies will move around the map, but we want the score to stay at the top left of the screen while the map scrolls around.

+   15. Below this, we now define the variables to hold our child components: