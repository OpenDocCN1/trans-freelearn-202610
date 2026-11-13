
## Instantiating animations and controlling them with Java code

This next snippet of Java code shows us how we would declare an object of the type Animation, initialize it with an animation contained in an XML file named fade_in.xml, and start the animation on ImageView. Soon, we will do this in a project and see where to put the XML animations as well:

```
// Declare an Animation object
Animation animFadeIn;

// Initialize it
animFadeIn = AnimationUtils.loadAnimation(getApplicationContext(),
R.anim.fade_in);

// Get an ImageView from the UI in the usual way
ImageView imageView = (ImageView) findViewById(R.id.imageView);

// Start the animation on the ImageView
imageView.startAnimation(animFadeIn);
```

We already have quite a powerful arsenal of animations and control features for things such as timing, but the Android API gives us a little bit more than this as well.

## More animation features

We can listen for the status of animations much like we can listen for clicks on a button. We can also use interpolators to make our animations more life-like and pleasing. Let's look at listeners first.

### Listeners

If we implement the `AnimationListener` interface, we can indeed listen to the status of animations by overriding the three methods that tell us when something has occurred. We could then take actions based on these events.

In the following code snippet, `onAnimationEnd` announces the end of an animation; `onAnimationRepeat` is called each and every time an animation begins a repeat, and perhaps predictably, `onAnimationStart` is called when an animation has started animating. This might not be the same time as when `startAnimation` is called if `startOffset` is set in the animation's XML:

```
@Override
public void onAnimationEnd(Animation animation) {
    // Take some action here
}

@Override
public void onAnimationRepeat(Animation animation) {
    // Take some action here
}

@Override
public void onAnimationStart(Animation animation) {
    // Take some action here
}
```

We will see how `AnimationListener` works in the Animations Demo app, as well as how to put another widget, **SeekBar**, into action.

### Animation interpolators

If you can think back to high school, you might remember really exciting lessons about calculating acceleration. If we animate something at a constant speed, then at first glance things might seem OK. If we then compare the animation to another that uses gradual acceleration, then the latter will almost certainly be more pleasing to watch. It is possible that if we were not told that the only difference between the two animations was that one used acceleration and the other didn't, we wouldn't be able to say why we preferred it. Our brains are more receptive to things that conform to the norms of the world around us. This is why adding a bit of real-world physics, such as acceleration and deceleration, improves our animations.

The last thing we want to do, however, is start performing a bunch of calculations just to slide a button onto the screen or spin a piece of text in a circle.

This is where interpolators come in. They are animation modifiers that we can set in a single line of code within our XML.

Some examples of interpolators are accelerate_interpolator and bounce_interpolator, as shown:

```
android:interpolator="@android:anim/accelerate_interpolator"
android:interpolator="@android:anim/cycle_interpolator"
```

We will put some interpolators along with some XML animations and the related Java code into action next.

## Tip

You can learn more about interpolators and the Android Animation class at the developer website,
http://developer.android.com/guide/topics/resources/animation-resource.html.

## The Animations Demo app – introducing SeekBar

That was enough theory, especially with something that should be so visible. Let's build an animation demo app that explores everything that we just discussed and a bit more.

This app involves a little amount of code in lots of different files. So, I have tried to make it plain which code is in which file, so you can keep track of what is going on. This will make the Java code that we write in this mini app more understandable as well.

The app will demonstrate rotations, fades, translations, animation events, interpolations, and controlling duration with SeekBar. The best way to explain what SeekBar does is to build it and then watch it in action.

## Laying out the animation demo

Create a new project called Animation Demo, leaving all the settings at their defaults as usual.

Here is how we can lay out the UI for this app:

- 1. Delete the default **Hello world!** widget.
- 2. Add **LinearLayout (vertical)**.
- 3. Inside the layout from the previous step, place **LinearLayout (vertical)** and **RelativeLayout**.
- 4. Set layout_weight on the two layouts that we added in the previous steps to .3 for LinearLayout and .7 for RelativeLayout.
- 5. Now, set layout_height of both to unset so that they fill three tenths and seven tenths of the screen, respectively.
- 6. For **LinearLayout (vertical)**, change its gravity property to center_horizontal.
- 7. Add **ImageView** to the inner **LinearLayout (vertical)**. Set the ImageView src property to @mipmap/ic_launcher to display the Android robot. Of course, you could choose any image that you have added to the drawable folder instead.
- 8. Set the id property of ImageView to imageView.
- 9. Directly below ImageView (still inside LinearLayout), add LargeText. Set the id property to textStatus and layout_marginTop to 10dp. This should look like this next screenshot:

![](img/35d01831c19688a44e50bce7c489007a_535_0.png)

- 10. Now, we will add a selection of **SmallButton** widgets to RelativeLayout. These are just the same as Buttons but with a smaller starting size. The exact positioning is not important, but the exact id property values that we add in a few steps time will be essential. Follow this next figure to lay out 12 buttons in RelativeLayout:

![](img/35d01831c19688a44e50bce7c489007a_536_0.png)

- 11. Add **SeekBar** on the left-hand side below the buttons. Set the id property to seekBarSpeed, the layout_width property to 250dp, and the max property to 5000. This means that the **SeekBar** widget will hold a value between 0 and 5000 as it is dragged by the user from left to right. We will see how we can read and use this data soon.

- 12. Add a plain **TextView** widget just on the right-hand side of **SeekBar** and set its id property to textSeekerSpeed.

- 13. The RelativeLayout portion of our design should now look like this:

![](img/35d01831c19688a44e50bce7c489007a_536_1.png)

- 14. Next, add the following id properties to the buttons, as identified by the text property that is already set:

| Existing text property | Value of the ID property to be set |
| :--- | :--- |
| FADE IN | btnFadeIn |
| FADE OUT | btnFadeOut |
| FADE IN OUT | btnFadeInOut |
| ZOOM IN | btnZoomIn |
| ZOOM OUT | btnZoomOut |
| LEFT RIGHT | btnLeftRight |
| RIGHT LEFT | btnRightLeft |
| TOP BOT | btnTopBottom |
| BOUNCE | btnBounce |
| FLASH | btnFlash |
| ROTATE LEFT | btnRotateLeft |
| ROTATE RIGHT | btnRotateRight |

We will see how to use this newcomer in our UI (SeekBar) when we get to coding MainActivity.

## Coding the XML animations

Right-click on the res folder and navigate to New | Android resource directory. Enter anim in the Directory name field and click on OK.

Now, right-click on the new anim directory and navigate to New | Animation resource file. In the File name field, type fade_in and then click on OK. Delete the entire contents and add this code to create the animation:

```
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
android:fillAfter="true" >

<alpha
android:fromAlpha="0.0"
android:interpolator="@android:anim/accelerate_interpolator"
android:toAlpha="1.0" >

</set>
```

Right-click on the anim directory and navigate to New | Animation resource file. In the File name field, type fade_out and then click on OK. Delete all the contents and add this code to create the animation:

```
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
android:fillAfter="true" >

<alpha
android:fromAlpha="1.0"
android:interpolator="@android:anim/accelerate_interpolator"
android:toAlpha="0.0" >

</set>
```

Right-click on the anim directory and navigate to New | Animation resource file. In the File name field, type fade_in_out and then click on OK. Delete all the contents and add this code to create the animation:

```
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
android:fillAfter="true" >
```
