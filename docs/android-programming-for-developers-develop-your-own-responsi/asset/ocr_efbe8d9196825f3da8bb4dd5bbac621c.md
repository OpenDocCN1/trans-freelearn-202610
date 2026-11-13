The fromXScale and toXScale values will increase or decrease the scale of an object. As an example, the code shown here will change the object that runs the animation from a normal size to invisible:

```
```xml
<scale
 android:fromXScale="1.0"
 android:fromYScale="0.0"
/>```
```

As another example, we could shrink the object to a tenth of its usual size using android:fromYScale="0.1" or make it 10 times as big using android:fromYScale="10.0".

# # Controlling the duration

Of course, none of these animations would be especially interesting if they just instantly arrived at their conclusion. To make our animations more interesting, we can, therefore, set their duration in milliseconds. A millisecond is one thousandth of a second. We can also make timing easier, especially in relation to other animations by setting startOffset, also in milliseconds.

The next code would begin an animation one-third of a second after we started it, and it would take two-thirds of a second to complete:

```
```
android:duration="666"
android:startOffset="333"
```
```

# # Rotating animations

If you want to spin something around, just use fromDegrees and toDegrees. This next code, predictably, will spin a widget around in a complete circle because, of course, there are 360 degrees in a circle:

```
```xml
<rotate android:fromDegrees="360"
 android:toDegrees="0"
/>```
```

# # Repeating animations

Repetition might be important in some animations, for example, a wobble or shake effect, so we can add a repeatCount property. In addition, we can specify how the animation is repeated by setting repeatMode.