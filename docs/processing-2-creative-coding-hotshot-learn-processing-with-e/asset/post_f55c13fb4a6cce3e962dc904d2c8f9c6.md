
- 2. We add a new tab and name it Visualizer. An interface is like a class definition without the method implementation. This interface defines the method that all our visualizer classes have to implement.

```java
interface Visualizer {
public int[][] tiles = new int[8][8];
public void init();
public void tick();
public void setActive( boolean active );
}
```

- 3. The first visualizer we are going to implement switches on and off after every second tile of our 8 x 8 tiles grid every time the tick() method gets called. Add a new tab, name it Alternate, and add the following code:

```java
public class Alternate implements Visualizer {
int p = 1;
int count = 0;

public Alternate() {
init();
}

public void tick() {
count++;
if ( count < 4 ) return;

count = 0;
if ( p == 1 ) { p = 0; } else { p = 1; }
for ( int x = 0; x < 8; x++ ) {
for ( int y = 0; y < 8; y++ ) {
tiles[x][y] = 255 * ( (x+y+p) % 2 ) ;
}
}
}

public void init() {
for ( int x = 0; x < 8; x++ ) {
for ( int y = 0; y < 8; y++ ) {
tiles[x][y] = 0;
}
}
}

public void setActive( boolean active ) {
}
}
```

- 4. Now switch back to the main sketch tab and add a variable for the current visualizer, and then initialize it in our setup() method.

```java
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

- 5. To get the tick method called every 125 milliseconds, we need to add a timer. Add the following thread to the setup() method:

```java
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

- 6. Our visualizer is initialized and changes the tile pattern every n milliseconds, but the tiles have not been drawn yet. We change this by adding the following code to our draw() method:

```java
void draw() {
  background(0);
  float w = 300/8;
  for ( int x = 0; x < 8; x++ ) {
    for ( int y = 0; y < 8; y++ ) {
      stroke( 255 ) ;
      fill( viz.tiles[ x ][ y ],0,0 ) ;
      if ( viz.tiles[ x ][ y ] > 32 ) {
        viz.tiles[ x ][ y ] -= (viz.tiles[ x ][ y ]-32)/25 ;
      }
      rect( w * x, w*y, w, w ) ;
    }
  }
}
```

The following screenshot shows what the pattern looks like:

![](img/5cb67baa7bc7778de360b14c00d1ab81_73_0.png)

- 7. If we start our sketch now, the tiles blink like mad, but to be honest, it gets a bit boring after a while. So let's add another tab and implement a new visualizer. We name the class BarHorizontal and add the following code:

```java
public class BarHorizontal implements Visualizer {
  int px = 0;
  int count = 0;

  public BarHorizontal() {
    init();
  }

  public void tick() {
    count++;
    if (count < 4) return;

    count = 0;
    for ( int i = 0; i < 8; i++ ) {
      tiles[min(7, px)][i] = 255;
    }

    if ( px >= 8 && tiles[7][7] != 0 ) {
      init();
    } else {
      px += 1;
    }
  }

  public void init() {
    px = 0;
    for ( int x = 0; x < 8; x++ ) {
      for ( int y = 0; y < 8; y++ ) {
        tiles[x][y] = 0;
      }
    }
  }

  public void setActive( boolean active ) {
  }
}
```

- 8. Switch back to the main tab and add an array for our visualizer classes. We also add a new timer similar to the first one, but this time we will make the delay longer, and instead of calling the tick() method we select a new visualizer from our array.

```java
Visualizer viz;
Visualizer[] visualizers;

Minim minim;
AudioPlayer player;
```

```java
void setup() {
    size(300,300);

    minim = new Minim( this );
    player = minim.loadFile( "loop.mp3", 1024 );

    visualizers = new Visualizer[] {
        new BarHorizontal(),
        new Alternate()
    };

    viz = visualizers[0];
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

    Thread t2 = new Thread() {
        public void run() {
            for(;;) {
                viz.setActive( false );
                viz = visualizers[ int(random(visualizers.length)) ];
                viz.init();
                viz.setActive( true );
                delay(8000);
            }
        }
    };
    t2.start();
    player.loop();
}
```

- 9. Now we are going to add a new visualizer that lights the panels in a spiral. We create a new tab, name it `SpiralIn`, and add the following code:

```java
public class SpiralIn implements Visualizer {
    PVector p = new PVector(0, 0);
    PVector d = new PVector(1, 0);

    public SpiralIn() {
        init();
    }

    public void tick() {
        tiles[int(p.x)][int(p.y)] = 255;
        if ( d.x == 1 && ( p.x == 7 ||
                tiles[int(p.x+1)][int(p.y)] != 0 ) ) {
            d = new PVector( 0, 1 );
        } else if ( d.x == -1 && ( p.x == 0 ||
                tiles[int(p.x-1)][int(p.y)] != 0 ) ) {
            d = new PVector( 0, -1 );
        } else if ( d.y == 1 && ( p.y == 7 ||
                tiles[int(p.x)][int(p.y+1)] != 0 ) ) {
            d = new PVector( -1, 0 );
        } else if ( d.y == -1 && ( p.y == 0 ||
                tiles[int(p.x)][int(p.y-1)] != 0 ) ) {
            d = new PVector( 1, 0 );
        }

        if ( p.x == 3 && p.y == 3 && tiles[3][4] != 0 ) {
            init();
        } else {
            p.add( d );
        }
    }

    public void init() {
        p = new PVector( 0, 0 );
        d = new PVector( 1, 0 );
        for ( int x = 0; x < 8; x++ ) {
            for ( int y = 0; y < 8; y++ ) {
                tiles[x][y] = 0;
            }
        }
    }

    public void setActive( boolean active ) {
    }
}
```

- 10. To make the new visualizer available in our sketch, we switch to the main sketch and add the SpiralIn class to the visualizers array:

```java
visualizers = new Visualizer[] {
    new SpiralIn(),
    new BarHorizontal(),
    new Alternate()
}
```

The pattern the SpiralIn visualizers generate is shown in the following screenshot:

![](img/5cb67baa7bc7778de360b14c00d1ab81_77_0.png)

- 11. Now we add a visualizer that isn't controlled by our `tick()` method, but instead by the samples from the `AudioPlayer` object. Our class implements the `Visualizer` interface and additionally the `AudioListener` interface from Minim. The `tick()` method stays empty in this visualizer because we calculate our tiles in the `samples()` method. We add a new tab, name it BeatVisualizer, and add the following code:

```java
public class BeatVisualizer implements Visualizer, AudioListener {
	boolean active = false;
	public BeatVisualizer() {
		init();
	}

	public void tick() {
	}

	public void init() {
		for ( int x = 0; x < 8; x++ ) {
			for ( int y = 0; y < 8; y++ ) {
				tiles[x][y] = 0;
			}
		}
	}

	void samples(float[] samp) {
		samples( samp, samp );
	}

	void samples(float[] sampL, float[] sampR) {
		if ( active ) {
			for ( int x = 0; x < 8; x++ ) {
				float val = 0;
				for ( int i = 0; i < 5; i++ ) {
					val = sampL[x*5+i] * 8;
				}

				for ( int y = -4; y < 4; y++ ) {
					if ( val > y && val < y+1 ) {
						tiles[x][4+y] = 255;
					} else {
						tiles[x][4+y] -= tiles[x][4+y] / 4;
					}
				}
			}
		}
	}

	public void setActive( boolean active ) {
	  this.active = active;
	}
}
```

- 12. Switch back to the main tab and add the BeatVisualizer class to the visualizers array. This time, we also need to register the visualizer as a callback function to the AudioPlayer object.

```java
void setup() {
  size(300,300);

  minim = new Minim( this );
  player = minim.loadFile( "loop.mp3", 1024 );

  BeatVisualizer bv = new BeatVisualizer();
  player.addListener( bv );

  visualizers = new Visualizer[] {
    new SpiralIn(),
    new BarHorizontal(),
    new Alternate(),
    bv
  };
}
```

The following screenshot shows the pattern that our BeatVisualizer class generates:

![](img/5cb67baa7bc7778de360b14c00d1ab81_79_0.png)
