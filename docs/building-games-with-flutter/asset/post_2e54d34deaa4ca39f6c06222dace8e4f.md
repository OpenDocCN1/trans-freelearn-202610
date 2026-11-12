
- 4. Next, we will add the constructor inside the class definition; this will include the values we need to pass to the `HudButtonComponent` base class via a call with `super`:

```dart
RunButton({
  required button,
  buttonDown,
  EdgeInsets? margin,
  Vector2? position,
  Vector2? size,
  Anchor anchor = Anchor.center,
  onPressed,
}) : super(
  button: button,
  buttonDown: buttonDown,
  margin: margin,
  position: position,
  size: size ?? button.size,
  anchor: anchor,
  onPressed: onPressed
);
```

- 5. Below this constructor, let's add a Boolean variable to keep track of whether this button is pressed or not, the default being `false`:

```dart
bool buttonPressed = false;
```

- 6. `HudButtonComponent` has a mixin called `Tappable` that allows us to detect when this button is pressed on the screen and override callbacks for `onTapUp`, `onTapDown`, and `onTapCancel`, which we will use to update the `buttonPressed` Boolean. Let's add these functions after the `buttonPressed` definition:

```dart
@override
bool onTapDown(TapDownInfo info) {
  super.onTapDown(info);
  buttonPressed = true;
  return true;
}

@override
bool onTapUp(TapUpInfo info) {
  super.onTapUp(info);
  buttonPressed = false;
  return false;
}

@override
bool onTapCancel() {
  super.onTapCancel();
  buttonPressed = false;
  return true;
}
```

We will use RunButton in the HUD component in the hud.dart file, but for now, let's move on to defining TextComponent for our score.

- 7. Open the score_text.dart file and add the following imports and class definition:

```dart
import 'package:flame/components.dart';
import 'package:flame/input.dart';
import 'package:flame/palette.dart';
import 'package:flutter/cupertino.dart';
class ScoreText extends HudMarginComponent {
}
```

Note here that the ScoreText class extends HudMarginComponent and not TextComponent. This is because we want to place TextComponent with margins around it. We will create TextComponent inside this class, add it as a child component of HudMarginComponent, and pass the margins into the class via the constructor, which we will add next.

- 8. Add the following constructor inside the class to pass the margins to the class:

```dart
ScoreText({EdgeInsets? margin}) : super(margin: margin);
```

- 9. Below the constructor, add the following variables for score and the TextComponent child:

```dart
int score = 0;
String scoreText = "Score: ";
late TextPaint _regularPaint;
late TextComponent scoreTextComponent;
```

- 10. Below these variables, let's add the `onLoad` function, which is called when the component is first used, to set up `TextComponent`:

```dart
@override
Future<void> onLoad() async {
  super.onLoad();
  TextStyle textStyle = TextStyle(color: BasicPalette.blue.color, fontSize: 30.0);
  _regularPaint = TextPaint(style: textStyle);
  scoreTextComponent = TextComponent(text: scoreText + score.toString(), textRenderer: _regularPaint);
  add(scoreTextComponent);
}
```

Here, we are setting the text color to blue and adding `scoreTextComponent` as a child to `ScoreText`.

- 11. Finally, for `TextComponent`, we want to expose a function for updating the score that will get updated every time George collides with one of the enemy sprites. So, let's add the `setScore` function below the `onLoad` function:

```dart
setScore(int score) {
  this.score += score;
  scoreTextComponent.text = scoreText + this.score.toString();
}
```

- 12. Next, we will add a simple `JoystickComponent` for controlling George's movement. To do that, open the `joystick.dart` file and add the following code:

```dart
import 'package:flame/components.dart';
import 'package:flutter/material.dart';
class Joystick extends JoystickComponent {
  Joystick({required PositionComponent knob, PositionComponent? background, EdgeInsets? margin}) : super(knob: knob, background: background, margin: margin);
}
```

We will keep this simple for now, but it's worth keeping JoystickComponent in its own file in case you want to style it more later, by using custom images for the joystick instead of colors.

- 13. Now that we have our joystick, run button, and score text components, let's create a HUD component that will handle the initialization of these components. First, open the `hud.dart` file and add the following imports and class definition:

```dart
import 'package:flame/components.dart';
import 'package:goldrush/components/hud/run_button.dart';
import 'package:goldrush/components/hud/score_text.dart';
import 'package:goldrush/components/hud/joystick.dart';
import 'package:flame/palette.dart';
import 'package:flutter/material.dart';
class HudComponent extends PositionComponent {
}
```

Here, we extend from PositionComponent, giving us control over how we arrange our child components with margins. PositionComponent is a base class that allows the positioning of components within it.

- 14. At the top of the HudComponent class, let's add a constructor that sets priority to 20:

```dart
HudComponent() : super(priority: 20);
```

priority is used to indicate the order components are drawn, with the default being 0. So, setting this higher than 0 will ensure that the HUD is drawn on top of everything else.

Later, when we start using a map that is larger than the physical screen, the character and enemies will move around the map, but we want the score to stay at the top left of the screen while the map scrolls around.

- 15. Below this, we now define the variables to hold our child components:

```dart
late Joystick joystick;
late RunButton runButton;
late ScoreText scoreText;
```

- 16. Now, set these child components up on the onLoad function:

```dart
@override
Future<void> onLoad() async {
  super.onLoad();
  final joystickKnobPaint = BasicPalette.blue.withAlpha(200).paint();
  final joystickBackgroundPaint = BasicPalette.blue.withAlpha(100).paint();
  final buttonRunPaint = BasicPalette.red.withAlpha(200).paint();
  final buttonDownRunPaint = BasicPalette.red.withAlpha(100).paint();
  joystick = Joystick(
    knob: CircleComponent(radius: 20.0, paint: joystickKnobPaint),
    background: CircleComponent(radius: 40.0, paint: joystickBackgroundPaint),
    margin: const EdgeInsets.only(left: 40, bottom: 40),
  );
  runButton = RunButton(
    button: CircleComponent(radius: 25.0, paint: buttonRunPaint),
    buttonDown: CircleComponent(radius: 25.0, paint: buttonDownRunPaint),
    margin: const EdgeInsets.only(right: 20, bottom: 50),
    onPressed: () => {}
  );
  scoreText = ScoreText(margin: const EdgeInsets.only(left: 40, top: 60));
  add(joystick);
  add(runButton);
  add(scoreText);
  positionType = PositionType.viewport;
}
```

Here, we set up some Paint objects for the colors of the components.

Next, we set up the joystick, using circles for the knob and background, and set the margin at the bottom left of the screen where the joystick will be located.

Then, we set up the run button in a similar way, with its margin at the bottom right to draw it opposite the joystick.

We then set the score text up with the margin at the top left of the screen.

After that, we add the HUD components and set `positionType` to `PositionType.viewport`.

`PositionType` ensures that this component is drawn on top of everything, even if the game camera moves around.

Please note that for the run button, it takes a function callback for when the button is pressed, but because we are managing the tap handling ourselves in the run button class, we can just ignore this and pass {} instead.

- 17. Let's add our `HudComponent` to the game so we can see our new components. Open the `main.dart` file and add the following import to the top of the file:

```dart
import 'package:goldrush/components/hud/hud.dart';
```

- 18. At the top of the `onLoad` function, let's add our `HudComponent`:

```dart
add(HudComponent());
```

- 19. As the `HudComponent` class contains components that are draggable (`Joystick`) and tappable (`RunButton`), we must pass the mixins for `HasDraggables` and `HasTappables` to the class definition for `MyGame`:

```dart
class MyGame extends FlameGame with HasCollidables, HasDraggables, HasTappables {
}
