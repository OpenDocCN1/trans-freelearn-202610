
- 6. Copy the two keys to a text file named keys.txt; place each key on a new line and make sure that there are no blank spaces at the end of each line.

- 7. For this task, we need the Twitter4J library. Go to http://twitter4j.org, download the twitter4j-3.0.3.zip file, and unzip it. We need the twitter4j-core.jar file from the lib folder of the ZIP package.

- 8. Create a new Processing sketch and add the jar file you just extracted from the ZIP file by dragging it to the sketch window or by using the Add File ... option under the Sketch menu.

- 9. Drag the keys.txt file we created in step 6 to the sketch window, or add it using the Add File ... option under the Sketch menu.

- 10. Now add a draw() and a setup() method to our sketch.

```
void setup() {

}

void draw() {
}
```

- 11. To use the Twitter4J library, we need to import it and define a twitter object. We also need to define a string array, which will contain some Twitter messages that we want to send.

```
import twitter4j.*;

Twitter twitter;
String[] tweets = {
  "tweet tweet #test",
  "test 1 2 3 #test",
  "microphone test #test"
};
```

- 12. In our setup() method, we need to initialize the twitter object and provide the consumer key and the consumer secret key that we stored in the keys.txt file. We also need an access token and an access token secret to get the permission to post to a user’s feed. To get these two access keys, we need to trigger a browser window to open to ask the user for his/her permission.

```
void setup() {
  size(300, 100);
  noLoop();
  textFont( createFont( "Georgia", 24 ) );

  String[] keys = loadStrings( "keys.txt" );

  try {
  twitter = TwitterFactory.getSingleton();
  twitter.setOAuthConsumer( keys[0], keys[1] );
  RequestToken requestToken =
  twitter.getOAuthRequestToken();
  AccessToken at = null;
  if ( System.getProperty("os.name").toLowerCase().
  indexOf("win")>=0) {
  link(requestToken.getAuthorizationURL());
  } else {
  open(requestToken.getAuthorizationURL());
  }
  } catch ( Exception e ) {
  e.printStackTrace();
  }
  }
```

This is the page that opens when our sketch doesn't find a key:

![](img/5cb67baa7bc7778de360b14c00d1ab81_109_0.png)

- 13. When the user clicks on **Authorize app**, Twitter generates a pin like the one shown in this screenshot:

![](img/5cb67baa7bc7778de360b14c00d1ab81_110_0.png)

- 14. Now we need the user to enter the pin from the website and use this pin to fetch our access token keys from Twitter.

```
void setup() {
  size(300, 100);
  noLoop();
  textFont( createFont( "Georgia", 24 ) );

  String[] keys = loadStrings( "keys.txt" );

  try {
    twitter = TwitterFactory.getSingleton();
    twitter.setOAuthConsumer( keys[0], keys[1] );
    RequestToken requestToken =
      twitter.getOAuthRequestToken();
    AccessToken at = null;
    if ( System.getProperty("os.name").toLowerCase().
        indexOf("win")>=0) {
      link(requestToken.getAuthorizationURL());
    }
    else {
      open(requestToken.getAuthorizationURL() );
    }

    String preset="";
    String pin=javax.swing.JOptionPane.showInputDialog(frame,
            "Enter your pin",preset);
    at = twitter.getOAuthAccessToken(requestToken, pin);

  } catch ( Exception e ) {
      e.printStackTrace();
  }

  }
```

- 15. Here you can see a screenshot of the dialog we created in the previous step:

![](img/5cb67baa7bc7778de360b14c00d1ab81_111_0.png)

- 16. We need to ask for the user's permission only once. If we already have access to the keys, we can reuse them. We store the consumer keys and the access token keys in the keys.txt file and use them when our sketch is run the next time.

```
void setup() {
    size(300,100);
    noLoop();
    textFont( createFont( "Georgia", 24 ) );

    String[] keys = loadStrings( "keys.txt" );
    try {
        twitter = TwitterFactory.getSingleton();
        twitter.setOAuthConsumer( keys[0], keys[1] );
        RequestToken requestToken= twitter.getOAuthRequestToken();
        AccessToken at = null;
        if ( System.getProperty("os.name").toLowerCase().
        indexOf("win")>=0 ) {
            link(requestToken.getAuthorizationURL());
        }
        else {
            open(requestToken.getAuthorizationURL());
        }
        String preset="";
        String pin=javax.swing.JOptionPane.showInputDialog(frame,
        "Enter your pin",preset);
        at = twitter.getOAuthAccessToken(requestToken, pin);
        String[] newKeys = new String[4];
        newKeys[0] = keys[0];
        newKeys[1] = keys[1];
        newKeys[2] = at.getToken();
        newKeys[3] = at.getTokenSecret();
        saveStrings( "data/keys.txt", newKeys );
    } catch ( Exception e ) {
        e.printStackTrace();
    }
}
```

- 17. If the keys.txt file already contains four keys because we already have the user's permission to tweet, we can use a configuration builder to create the Twitter object.

```
void setup() {
size(300,100);
noLoop();
textFont( createFont( "Georgia", 24 ) );
String[] keys = loadStrings( "keys.txt" );
if (keys.length <= 2) {
try {
twitter = TwitterFactory.getSingleton();
twitter.setOAuthConsumer( keys[0], keys[1] );
RequestToken requestToken = twitter.getOAuthRequestToken();
AccessToken at = null;
if ( System.getProperty("os.name").toLowerCase() .
indexOf("win")>=0) {
link(requestToken.getAuthorizationURL());
} else {
open(requestToken.getAuthorizationURL());
}

String preset="";
String pin=javax.swing.JOptionPane.showInputDialog(frame,
"Enter your pin",preset);
at = twitter.getOAuthAccessToken(requestToken, pin);

String[] newKeys = new String[4];
newKeys[0] = keys[0];
newKeys[1] = keys[1];
newKeys[2] = at.getToken();
newKeys[3] = at.getTokenSecret();

saveStrings( "data/keys.txt", newKeys );
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

- 18. In our draw() method, we set the background and fill color and display a text message to make sure the user knows what to do.

```
void draw() {
background(255);
fill(0);
text( "click here to tweet", 40, 60 );
}
```

- 19. Now we add a mouseClicked() method to our sketch and create a StatusUpdate object. We choose a random tweet from the list of messages we created at the beginning and use the updateStatus() method of our Twitter object to send it.

```
void mouseClicked() {
try {
StatusUpdate status = new StatusUpdate(
tweets[ int(random( tweets.length )) ] );
twitter.updateStatus( status );
} catch ( Exception e ) {
e.printStackTrace();
}
}
```

- 20. Run the sketch and click on the window to generate some tweets. This is what the application window looks like:

![](img/5cb67baa7bc7778de360b14c00d1ab81_114_0.png)

## Objective Complete - Mini Debriefing

Task 3 of our current mission was to implement a tweeting sketch. In steps 1 to 5, we registered our application on the Twitter developer site and generated the required consumer and consumer secret keys. To actually post on behalf of a Twitter user, we need two more keys: an access token and an access token secret. To get these, we opened a browser window in step 12.

After the user grants the requested permission to our application, we need the pin that is shown in the browser window to finish our authorization process. We therefore open a swing input dialog and ask the user to enter the pin.

The access token keys are reusable, so we only need to acquire them once and then store them to the same text file where our consumer keys are kept. If this file already contains the four keys, we can create our Twitter objects directly and start tweeting right away.

We used Twitter4J to access the Twitter API, and to actually tweet something, we implemented a mouseClicked() method, which sends a random tweet to the world. We defined an array of strings for these tweets and chose a random one every time, because Twitter doesn't allow the exact same text to be posted twice via the API (for spam prevention).

## Tweet your mood

The final task in this mission is to combine tweeting with the mood faces we created earlier to tell the world how we currently feel. We extend the GUI section of our Smilie-O-Mat with a button and implement a status update method that doesn't just tweet a text message, but also uploads our current smiley.
