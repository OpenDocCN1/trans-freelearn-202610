```
String[] rawLines = loadStrings
( "romeo_and_juliet.txt" );
ArrayList lines = new ArrayList();
for ( int i=0; i<rawLines.length; i++ ) {
if ( !"".equals(rawLines[i]) ) {
String[] tmp = rawLines[i].split( "#" );
lines.add( new Line( tmp[0], tmp[1].trim() ) );
}
}

TTS romeo = new TTS();
TTS juliet = new TTS();
juliet.setPitchShift( 2.4 );
drama = new Drama( lines, romeo, juliet );
}
```

+   2. Switch to the Drama thread and add two variables to our actors.

```
public class Drama extends Thread {
TTS romeo;
TTS juliet;

int current;
ArrayList lines;
boolean running;
...
```

+   3. We also need to extend the constructor of our Drama class to enable us to add the actors.

```
public Drama( ArrayList lines, TTS romeo, TTS juliet ) {
this.lines = lines;
this.romeo = romeo;
this.juliet = juliet;
current = 0;
running = false;
}
```

+   4. In the run() method, we take the current line and choose the actor object depending on the actor variable we added to each line. We also don't need the delay() and the println() methods anymore.

```
public void run() {
running = true;

for ( int i=0; i < lines.size(); i++ ) {
current = i;
Line l = (Line)lines.get(i);
```