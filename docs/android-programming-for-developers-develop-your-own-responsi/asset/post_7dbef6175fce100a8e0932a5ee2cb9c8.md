
## Do you want to use 3D?

If the answer is yes, I would definitely recommend you to use an already existing engine. There are some well-known tasks you'll need to implement to build even the simplest 3D engine such as loading models, loading and applying textures, handling transformations, and dealing with cameras. On top of this, you'd need to be writing OpenGL. All this is a lot of work.

Writing an OpenGL engine is the very definition of reinventing the wheel. It is fine if what you want is to learn the internals of a 3D engine, but if you go this road you'll spend a few months before you can even start with the game. If you want to go straight into making the game, you'd better start with an existing 3D engine.

The second question on this road is: do you prefer to work with code or are you more comfortable with a complete editor? For code, you can use jPCT-AE and libGDX, while, on the editor side, the most common alternative is Unity.

## Do you want to use physics?

An affirmative answer to this question should point you straight to an existing engine.

Physics simulation is a very well-known area where there is a lot of documentation, and you should be able to implement your own physics engine. Again, this is a great learning experience, but if you want to go straight into making the game it is much more convenient to use an existing engine that supports physics. The most used physics engine around is Box2D, which is written in C++ and it has been ported to Android using the NDK.

While we are going to talk about collision detection later in the book, physics is beyond the scope of this book. Anything more complex than two spheres colliding can become quite complex to handle.

Once again, it depends whether you prefer to work with code or if you want a complete editor. To work with code, AndEngine should be your weapon of choice. In the case of an editor, Corona and Unity are among the most popular choices.

## Do you want to use Java?

Most of the feature-rich environments we are mentioning have their own environment, including a specific IDE. It takes effort to learn them and some of them use a different language (for example Unity has its own environment and uses JavaScript or C#).

On the other hand, the frameworks are simpler. You just have to include them and you'll still be writing an Android game. This is an interesting middle ground, where you still can reuse your Android and Java knowledge and make use of features such as physics or 3D models. In this section, we can mention AndEngine for 2D and physics and jPCT-AE for 3D as good options.

## Pros of building games with the Android SDK

There are several advantages to building games using the Android SDK:

- It is faster to build a prototype
- You have full control over the engine
- It has a smaller learning curve (you already know Android, Java, and Android Studio)
- Most of your knowledge can be applied to apps
- You can use Google Play services and other libraries natively

## Cons of building games with the Android SDK

Of course, not everything is awesome. There are some serious disadvantages, most of them already mentioned, such as:

- The code is not portable to other platforms (namely iOS).
- Performance can be an issue. If the game gets to a certain complexity, you may need to use OpenGL.
- It lacks a physics engine; you'd need to write it yourself.
- The support for OpenGL is just primitives; you need to build everything (or use a library).

## I want the Android SDK!

Are you still here? Congratulations, you have chosen the right book!

If you want to explore other options, there are books available for Unity, AndEngine, and libGDX, and published by Packt.

Now that we are all on the same page, let's get down to business.

## The project – YASS (Yet Another Space Shooter)

Along the book, we will be building a game as a demo of the concepts we will be studying in each chapter. The game is going to be a classic Space Shooter arcade game. We'll call it YASS—Yet Another Space Shooter.

This means some decisions will be taken for this particular type of game, but other options will also be commented since the book is meant for generic video game development.

## Activities and Fragments

We are going to create a project with a single Activity and we will add fragments when necessary.

In the versions prior to Android 5.0 Lollipop, the transitions between activities could be modified, but only in a very limited way. The user can even disable them in a setting. All in all, this will make your game look clunky while transitioning from one Activity to another. You will need to save the state of the Activity in case it gets destroyed. Since each Activity is a separate instance, you will need to take care of communication among them, if required.

On the other hand, when you work with fragments, you never exit the Activity and you have complete control over the transition animations. In addition to these, you still have the code and layout of each section separated, so modularity and encapsulation are not compromised.

Finally, when it comes to handling third-party libraries such as In-App Billing or Google Play services, you have to take care of initialization and configuration only once, since those are linked at the Activity level.

### Note

For games, it is more efficient to use only one Activity with multiple Fragments.

One good practice is to have a base Fragment for our game (YassBaseFragment) from which all the other fragments will inherit. One good use of this fragment is to have a method to replace getActivity that returns our specific Activity, but there are other cases in which having a common base fragment is handy.

## Project setup

We are going to use Android Studio as the IDE. We are going to create the project with minSDK 15 (Ice Cream Sandwich—ICS). As a good practice, we don't want to move the minimum SDK, unless we are using some features that were not available before. By keeping the minSDK low, you make your game available to as many devices as possible.

The two main features we are going to use from ICS are Fragments, ValueAnimators, and ViewPropertyAnimators. All of these were already available in Honeycomb, but 3.x is considered little more than a test for ICS; it was not mature and has been replaced by ICS in almost all devices.

In the unlikely case that you want to support older versions such as Gingerbread, you can make use of the compatibility library and NineOldAndroids to add backwards-compatibility for the features we are using.

### Creating the stub project

Let's go on and navigate to File > New Project. We are going to use YASS as the Application name and example.com as the Company Domain.

![](img/35d01831c19688a44e50bce7c489007a_1201_0.png)

We include support for Android TV, since we want to be able to run our game on the big screen. This will create an extra module that we can compile for, but we are not going to touch this until the last chapter.

As explained before, we will use **Minimum SDK** version 15 for phones and 21 for Android TV, since this is when it was made available.

For the **Package name** of the application, we are going to use `com.example.yass`.

![](img/35d01831c19688a44e50bce7c489007a_1202_0.png)

We are not going to use any of the default wizards, since all of them include the action bar/toolbar that is great for apps, but of no use for games. So, we'll go with the empty project options:

![](img/35d01831c19688a44e50bce7c489007a_1203_0.png)

Similarly, we are not going to create any Activity for TV:

![](img/35d01831c19688a44e50bce7c489007a_1204_0.png)

Once the project is created, we will create a single Activity with one Fragment. This is done via the menu option **New > Activity > Blank Activity with Fragment**.

![](img/35d01831c19688a44e50bce7c489007a_1205_0.png)

We are going to customize the Activity by filling the dialog as follows:

- **Activity Name**: YassActivity
- **Layout Name**: activity_yass (will be the default as soon as we change the Activity name)
- **Fragment Layout Name**: fragment_yass (will be the default as soon as we change the Activity name)
- **Title**: YassActivity

This will create the following files:

- YassActivity.java with the code for YassActivity and PlaceholderFragment
- activity_main.xml: A FrameLayout with @+id/container, which will be used to load the fragments into
