# Coding the Sound Demo app

To code the Sound Demo app, we first add a few member variables for our SoundPool, sound FX IDs, and nowPlaying int as previously discussed, and we will also add float to hold a value for the volume between 0 (silent) and 1 (full volume relative to the current volume of the device). We will also add int called repeats, which unsurprisingly holds the value of the number of times we will repeat a given sound FX:

```
SoundPool sp;

int idFX1 = -1;
int idFX2 = -1;
int idFX3 = -1;
int nowPlaying = -1;

float volume = .1f;
int repeats = 2;
```

Now, in onCreate, we can get a reference and set a click listener for our buttons in the usual way:

```
Button buttonFX1 = (Button) findViewById(R.id.btnFX1);
buttonFX1.setOnClickListener(this);

Button buttonFX2 = (Button) findViewById(R.id.btnFX2);
buttonFX2.setOnClickListener(this);

Button buttonFX3 = (Button) findViewById(R.id.btnFX3);
buttonFX3.setOnClickListener(this);

Button buttonStop = (Button) findViewById(R.id.btnStop);
buttonStop.setOnClickListener(this);
```

Still, in onCreate, we can initialize SoundPool (sp) based on the version of Android that the device is using:

```
// Instantiate our sound pool dependent // upon which version of
Android
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
 AudioAttributes audioAttributes = new AudioAttributes.Builder()
 .setUsage(AudioAttributes.USAGE_ASSISTANCE_SONIFICATION)
 .setContentType(AudioAttributes.CONTENT_TYPE_SONIFICATION)
 .build();
}
```