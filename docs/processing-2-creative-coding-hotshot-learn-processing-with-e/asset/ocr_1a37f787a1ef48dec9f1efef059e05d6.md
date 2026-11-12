```
}
}
```

```
public void setActive( boolean active ) {
  this.active = active;
}
}
```

+   12. Switch back to the main tab and add the BeatVisualizer class to the visualizers array. This time, we also need to register the visualizer as a callback function to the AudioPlayer object.

```
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

![](bbox=[0.358, 0.6, 0.642, 0.814])