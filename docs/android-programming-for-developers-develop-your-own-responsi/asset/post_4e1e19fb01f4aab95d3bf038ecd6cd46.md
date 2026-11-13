
## Enabling the user to switch to the Settings screen

We already know how to switch to SettingsActivity. Also, as we won't be passing any data to it or from it, we can get this working with just two lines of Java.

You might have noticed that in the action bar of our app, there is the menu icon.

![](img/35d01831c19688a44e50bce7c489007a_501_0.png)

It is indicated in the following image:

If you tap it, there is already a menu option in there for **Settings**, provided by default when we first created the app. And all we need to do is place our code to switch to SettingsActivity within the onOptionsItemSelected method. Android Studio even provided an if block for us to paste our code into with the assumption we would one day want to add a Settings menu. How thoughtful!

Find the following block of code in the onOptionsItemSelected method in MainActivity.java:

```
//noinspection SimplifiableIfStatement
if (id == R.id.action_settings) {
    return true;
}
```

Add this code into the if block shown previously, just before the return true statement:

```
Intent intent = new Intent(this, SettingsActivity.class);
startActivity(intent);
```

You can now run the app and visit the new **Settings** screen by tapping the **Settings** screen's menu option.

To return from SettingsActivity to MainActivity, you can tap the back button on the device. We will be making this a bit more intuitive for the user later in the chapter in the section Specifying a parent Activity.

## Persisting data with SharedPreferences

In Android, there are a number of ways to make data persist. By persist, I mean that if the user quits the app, when they come back to it, their data is still available. The correct method to use is dependent upon the app and type of data.

In this book, we will look at three ways to make data persist. For saving our users' settings, we only need a really simple method. After all, we just need to know if they want sound and at which speed they want their animations.

Let's take a look at how we can make our apps save and reload variables to the internal storage of the device. We need to use the SharedPreferences class. SharedPreferences is a class that provides access to data that can be accessed and edited by all activities of an app. Let's look at how we can use it:

```
// A SharedPreferences for reading data
SharedPreferences prefs;
```

```
// A SharedPreferences.Editor for writing data
SharedPreferences.Editor editor;
```

As with all objects, we need to initialize them before we can use them. We can initialize the prefs object using the getSharedPreferences method and by passing in String, which will be used to refer to all the data read and written using this object. Typically, we could use the name of the app as this string. In the next code, MODE_PRIVATE means that any class, in this app only, can access it:

```
prefs = getSharedPreferences("My App", MODE_PRIVATE);
```

We then use our newly initialized prefs object to initialize our editor object by calling the edit method:

```
editor = prefs.edit();
```

Let's say we wanted to save the user's name that we have in a string called username. We can then write the data to the internal memory of the device like this:

```
editor.putString("username", username);
```

```
editor.putString("username", username);
editor.commit();
```

The first argument used in the putString method is a label that can be used to refer to the data, and the second is the actual variable that holds the data we want to save. The second line in the previous code initiates the saving process. So, we could write multiple variables to disk like this:

```
editor.putString("username", username);
editor.putInt("age", age);
editor.putBoolean("newsletter-subscriber", subscribed);

// Save all the above data
editor.commit();
```

The preceding code demonstrates that you can save other variable types, and it of course assumes that the username, age, and subscribed variables have previously been initialized with appropriate values.

Once editor.commit() is executed, the data is stored. We can quit the app and even turn off the device and the data will persist.

Let's see how we can reload our data the next time the app is run. This code will reload the three values that the previous code saved. We could even declare our variables and initialize them with the stored values:

```
String username = prefs.getString("username", "new user");

int age = prefs.getInt("age", -1);

boolean subscribed = prefs.getBoolean("newsletter-subscriber", false)
```

In the previous code, we load the data from disk using the method appropriate for the data type and the same label we used to save the data in the first place. What is less clear is the second argument to each of the method calls. The getString, getInt, and getBoolean methods require a default value as the second parameter. If there is no data stored with that label, it will then return the default value. We could then check for these default values in our code and go about trying to obtain the real values. For example:

```
if (age == -1){
    // Ask the user for his age
}
```

We now know enough to save our users' settings in the Note To Self app.

## Making the Note To Self app's settings persist

We have already learned how to save data to the device's memory. As we implement saving users' settings, we will also see again how we handle the CheckBox, RadioGroup, and RadioButton input.

## Coding the SettingsActivity class

Most of the action will take place in the SettingsActivity.java file. So, click on the appropriate tab and we will add the code a bit at a time.

First, we need some member variables that will give us a working SharedPreferences and Editor reference. We also want a member variable to represent each of the users' settings options. We need one for sound (boolean for on or off) and int for their preferred animation option (fast, slow, or none). Furthermore, as you will see in the next code, we have three member variables: public, static, and final. These serve several purposes.

As they are final, they cannot be changed, so they are useful for remembering the three possible states the animation option can be set to. This is a lot easier than trying to remember which value represents which animation state. In addition, these variables are static.

### Tip

Reminder: Static variables persist and are the same throughout multiple instances of a class. They will be accessible in MainActivity as well as SettingsActivity, even when an instance of SettingsActivity does not exist. We will soon see how this is useful.

Add the following member variables to SettingsActivity:

```
private SharedPreferences mPrefs;
private SharedPreferences.Editor mEditor;

private boolean mSound;

public static final int FAST = 0;
public static final int SLOW = 1;
public static final int NONE = 2;

private int mAnimOption;
```

Now, in onCreate, add the highlighted code to initialize mPrefs and mEditor:

```
mPrefs = getSharedPreferences("Note to self", MODE_PRIVATE);
mEditor = mPrefs.edit();
```

Next, still in onCreate, let's get a reference to our checkbox and load up the saved data that represents our user's previous choice for sound. We get a reference to the checkbox in the usual way and based on the value of mSound, we either check or uncheck the box:

```
mSound = mPrefs.getBoolean("sound", true);

CheckBox checkBoxSound = (CheckBox) findViewById(R.id.checkBoxSound);

if(mSound){
 checkBoxSound.setChecked(true);
}else{
 checkBoxSound.setChecked(false);
}
```

Next, we make an anonymous class to handle any changes to the sound options by detecting taps on the CheckBox class.

Notice the onCheckedChanged method has a Boolean parameter, isChecked. We add some logging code so we can see what is going on in the console, but the real functionality is simple. Change the status of mSound by reversing it, by making it ! (not) what it was previously. Then, use Editor to put the new value into the sound key-value pair.

Add the following code to the onCreate method that we have just discussed:

```
checkBoxSound.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener()
{
 public void onCheckedChanged(CompoundButton buttonView, boolean isChecked)
 {
   Log.i("sound = ", "" + mSound);
   Log.i("isChecked = ", "" + isChecked);
   // If mSound is true make it false
   // If mSound is false make it true
   mSound = ! mSound;
   mEditor.putBoolean("sound", mSound);
 }
});
```
