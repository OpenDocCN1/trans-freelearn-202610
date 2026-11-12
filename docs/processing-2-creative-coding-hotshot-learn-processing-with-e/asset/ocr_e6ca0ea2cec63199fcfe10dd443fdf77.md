15. Now add a little bit of text to the `draw()` method to tell the user what to do. Add the following code to the `draw()` method:

```
void draw() {
  background(255);
  textAlign(CENTER);
  fill(0);

  text( "Click here for Drama", width/2, height/2 );
}
```

16. Currently, our sketch window is way too small to contain the text, and we also want to use a bigger font. To change the window size, we simply add the following line to the `setup()` method:

```
void setup() {
  size( 800, 400 );

  String[] rawLines = loadStrings
  ( "romeo_and_juliet.txt" );
  ArrayList lines = new ArrayList();
  for (int i=0; i<rawLines.length; i++) {
    if ("".equals(rawLines[i])) {
      String[] tmp = rawLines[i].split("#");
      lines.add( new Line( tmp[0], tmp[1].trim() ) );
    }
  }

  drama = new Drama( lines );
}
```

17. To change the used font, we need to tell Processing which font to use. The easiest way to find out the names of the fonts that are currently installed on the computer is to create a new sketch, type the following line, and run the sketch:

```
println(PFont.list());
```

18. Copy one of the font names you like and add the following line to the Romeo and Juliet sketch:

```
void setup() {
  size( 800, 400 );
  textFont( createFont( "Georgia", 24 ) );

  ...
}
```

19. Replace the font name in the code lines with one of the fonts on your computer.