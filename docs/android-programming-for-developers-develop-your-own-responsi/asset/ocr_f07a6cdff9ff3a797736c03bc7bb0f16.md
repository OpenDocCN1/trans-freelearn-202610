# # Coding the SettingsActivity class

Most of the action will take place in the SettingsActivity.java file. So, click on the appropriate tab and we will add the code a bit at a time.

First, we need some member variables that will give us a working SharedPreferences and Editor reference. We also want a member variable to represent each of the users' settings options. We need one for sound (boolean for on or off) and int for their preferred animation option (fast, slow, or none). Furthermore, as you will see in the next code, we have three member variables: public, static, and final. These serve several purposes.

As they are final, they cannot be changed, so they are useful for remembering the three possible states the animation option can be set to. This is a lot easier than trying to remember which value represents which animation state. In addition, these variables are static.

# ## Tip

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