# # Specifying a parent Activity

It is not essential but it is good practice (with benefits) to declare a parent Activity for new activities. By doing so, we will also enhance our app visually and improve its usability as well.

Open up the AndroidManifest.xml file from the manifests folder. Add the highlighted line of the following code in the position shown:

```
...
<activity
    android:name=".SettingsActivity"
    android:parentActivityName=".MainActivity"
    android:label="@string/title_activity_settings" >
</activity>
...
```

SettingsActivity now officially has MainActivity as its parent. Now, we can run the app and see a minor visual enhancement. The user can now use the device's back button or the icon added to the UI upon adding that last bit of code:

![](bbox=[0.11, 0.5, 0.59, 0.76])

Now we have a neat Settings screen, and we can permanently save the user's choices. Of course, the big missing link with regard to persistence is that the user's fundamental data, his or her notes, still do not persist.