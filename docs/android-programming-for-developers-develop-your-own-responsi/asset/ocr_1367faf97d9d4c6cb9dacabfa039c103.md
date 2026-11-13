![](bbox=[0.112, 0.102, 0.883, 0.163])

The spaceship set made by Eikesteer that we picked from Open Game Art

From the set, we will use the the third from the right. We can extract it to a new image using a simple editor such as GIMP and place it under the drawable-nodpi directory.

Note that we are going to scale everything to be consistent with our 400 units of screen height, so it does not make sense to put the image in a drawable directory that has a density qualifier. This is why we are going to use drawable-nodpi.

The drawable-nodpi directory is meant to be independent from any density, while drawable is meant for images that do not have a qualifier. This means that the behavior is different when we try to read the intrinsic size of a drawable image. The intrinsic size will return the real size when placed under nodpi and will depend on the device when read from drawable.

# Note

We will place our game object images in the drawable-nodpi folder.

The next step is to create an ImageView to display our spaceship. We are going to do this inside the constructor of the Player object:

```
public Player(final View view) {
[...]
// We create an image view and add it to the view
mShip = new ImageView(view.getContext());
Drawable shipDrawable = view.getContext().getResources()
        .getDrawable(R.drawable.ship);
mShip.setLayoutParams(new ViewGroup.LayoutParams(
        (int) (shipDrawable.getIntrinsicWidth()  mPixelFactor),
        (int) (shipDrawable.getIntrinsicHeight() mPixelFactor)));
mShip.setImageDrawable(shipDrawable);
```