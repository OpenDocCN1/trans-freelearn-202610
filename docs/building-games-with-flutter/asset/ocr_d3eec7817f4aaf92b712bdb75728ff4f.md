# # Playing sound effects

As mentioned in the previous section, when playing sound effects, we need to handle the pausing and resuming of the sound effects if they are still playing when the app is put in the background, for instance, to check something else on your phone, as this is not currently handled by the library.

We will initially update our `Character` class, which is our top-level base class for all our sprites, to add `onPaused` and `onResumed` callbacks, which all our sprites can use.

We will then listen for life cycle change events in our game and if these are called, we will iterate over all our sprites and pass on these events.

And finally, as the sound effects are related to George, we will update the `George` class to play sounds and pause and resume these sound effects when needed. Let's get started:

+   1. Open up the `character.dart` file. At the bottom of the `Character` class, add the following function definitions:

```
void onPaused() {}
void onResumed() {}
```

Note that we will leave these functions empty as we will override these in the `George` class.

+   2. Open the `main.dart` file, and below the `onRemove` function that we recently added, add the following code to listen and handle life cycle events:

```
@override
void lifecycleStateChange(AppLifecycleState state) {
    switch(state) {
        case AppLifecycleState.paused:
            children.forEach((component) {
                if (component is Character) {
                    component.onPaused();
                }
            }
    }
```