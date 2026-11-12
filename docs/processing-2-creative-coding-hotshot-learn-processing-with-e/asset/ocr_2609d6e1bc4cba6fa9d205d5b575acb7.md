4. Now switch back to the main sketch tab and add a variable for the current visualizer, and then initialize it in our setup() method.

```
Visualizer viz;
Minim minim;
AudioPlayer player;

void setup() {
  size(300,300);

  minim = new Minim( this );
  player = minim.loadFile( "loop.mp3", 1024 );

  viz = new Alternate();
  viz.setActive( true );

  player.loop();
}
```

5. To get the tick method called every 125 milliseconds, we need to add a timer. Add the following thread to the setup() method:

```
Visualizer viz;
Minim minim;
AudioPlayer player;

void setup() {
  size(300,300);

  minim = new Minim( this );
  player = minim.loadFile( "loop.mp3", 1024 );

  viz = new Alternate();
  viz.setActive( true );
  Thread t = new Thread() {
    public void run() {
      for(;;) {
        viz.tick();
        delay(125);
      }
    }
  };
  t.start();

  player.loop();
}
```