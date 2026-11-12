```
newKeys[3] = at.getTokenSecret();

saveStrings( "data/keys.txt", newKeys  );

} catch ( Exception e ) {
    e.printStackTrace();
}

} else {
ConfigurationBuilder cb = new ConfigurationBuilder();
cb.setOAuthConsumerKey( keys[0] );
cb.setOAuthConsumerSecret( keys[1] );
cb.setOAuthAccessToken( keys[2] );
cb.setOAuthAccessTokenSecret( keys[3] );

twitter = new TwitterFactory( cb.build() ).getInstance();
}
}
```

+   11. Now we call the initTwitter() method from our setup() method.

```
void setup() {
size(300,390);
smooth();
background(255);
colorMode(HSB);
textFont( createFont( "Georgia", 20 ) );

initTwitter();
}
```

+   12. To tweet the current smiley, we add a new method named tweet(). Since we only want to tweet the image of the smiley and not the GUI elements, we need to copy the section of the window we want to a PImage object and then save it to a file. We will use this file as a media object for our status updates to upload to Twitter.

```
void tweet() {

PIimage image = get( 0, 0, 300,300 );
image.save( "smilie.png" );

java.io.File f = new java.io.File( sketchPath( "smilie.png" ) );
StatusUpdate status = new StatusUpdate( tweets[int(random(
tweets.length ))]);
status.setMedia(f);

try {
twitter.updateStatus(status);
}
```