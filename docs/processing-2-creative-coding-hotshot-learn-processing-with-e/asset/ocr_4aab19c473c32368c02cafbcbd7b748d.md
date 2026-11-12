# Classified Intel

FreeTTS and ttslib also allow you to use a binary TTS engine named MBROLA. Unfortunately, it's only distributed in binary form, and at the time of writing, it only works on Linux. So if you are using Linux and want to give it a try, you can make the following changes to our Romeo and Juliet sketch:

+   1. Open http://ctts.fpms.ac.be/synthesis/mbrola.html in your browser and click on Download. Download the MBROLA binary for your platform.
+   2. Download the female_us1 voice from the MBROLA site.
+   3. Create a folder for the MBROLA binary and unzip the two packages you just downloaded. Make sure that the path to the MBROLA binary contains no blanks, since FreeTTS can't deal with it.
+   4. Rename the MBROLA binary to mbrola.
+   5. Now go back to your Romeo and Juliet sketch and add the following highlighted line to your setup() method:

```
void setup() {
  System.setProperty("mbrola.base", "/path/to/mbrola/");

  size( 800, 400 );
  textFont( createFont( "Georgia", 24 ) );

  String[] rawLines = loadStrings
    ( "romeo_and_juliet.txt" );
  ArrayList lines = new ArrayList();
  for ( int i=0; i<rawLines.length; i++) {
    if ("!=".equals(rawLines[i])) {
      String[] tmp = rawLines[i].split("#");
      lines.add( new Line( tmp[0], tmp[1].trim() ) );
    }
  }

  drama = new Drama( lines );
  TTS romeo = new TTS();
  TTS juliet = new TTS();
  juliet.setPitchShift( 2.4 );
  drama = new Drama( lines, romeo, juliet );
}
```