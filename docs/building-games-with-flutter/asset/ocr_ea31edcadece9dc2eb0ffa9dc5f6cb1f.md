```
child: Slider(
    value: musicVolume,
    min: 0.0,
    max: 100.0,
    label: '₍${musicVolume.round()}₎',
    divisions: 4,
    onChanged: (double newMusicVolume) {
      SharedPreferences.getInstance().then((prefs)
        => prefs.setDouble('musicVolume',
        newMusicVolume));
      setState(() => musicVolume = newMusicVolume);
    }),
  );
}
```

Here, we return a Slider widget wrapped around a fixed SizedBox widget that has values for the music volume between 0 and 100. Whenever the slider value is changed, we set the musicVolume value in the widget and save this to the shared preferences.

+   4. The following is the code for the getBackLabel function:

```
Widget getBackLabel() {
  return GestureDetector(
    onTap: () { Navigator.pushNamedAndRemoveUntil(
      context, "/", (r) => false); },
    child: Text('Back', style: TextStyle(color:
      Colors.red, fontSize: 32.0))
  );
}
```

Here, we return a Text label to go back to the main menu and use Navigator to return the player to the top-level route if this is clicked.

The following screenshot shows our settings screen along with a slider to set the music volume: