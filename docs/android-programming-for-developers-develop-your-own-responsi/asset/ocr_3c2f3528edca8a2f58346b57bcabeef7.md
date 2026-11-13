# # Coding MainActivity

Most of the action will take place in the next class we create. We will call that class GameView. So, the job of MainActivity is to communicate with the lifecycle events of the OS and pass on any relevant information to GameView. It needs to instantiate a GameView object as well. Notice in the next code there is indeed an object of the type GameView declared as a member.

In the onCreate method, we use a Display object and the getWindowManager().getDefaultDisplay() chained methods to initialize it. Then, we declare an object of the type Point. Using the Display object, we can load the screen resolution into point using the getSize method.

We can now call the constructor of GameView to initialize gameView. Notice when we do that we pass in x and y, which is the screen resolution. It is from here that our Paddle and Ball objects will eventually get hold of them.

Finally, in onCreate, we do something that might at first look slightly odd. Look at the call to setContentView. Instead of passing in the XML layout file, we pass in our GameView reference.

This is exactly what we need. GameView will extend a class called SurfaceView, which not only allows us to have a Thread, but also implements onTouchListener and allows us to attach a Canvas object directly to it.

So, GameView, because it extends SurfaceView, is like an all-in-one solution to our thread, touching, and drawing needs, and we simply set it as the view of our entire Activity.

Add the highlighted member variable and code to onCreate:

```
// gameView will be the view of the game
// It will also hold the logic of the game
// and respond to screen touches as well
GameView gameView;
```

```
@Override
protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
```