It makes sense that these bundles be configured (packed with data) in the same place and in the same method of the interface so that the Bundle passed into a new Activity that we know is destined for a particular Fragment in the end, will be managed from the same place.

We will see this happen in the Address book app very soon, but you will probably be pleased to hear that once we have prepared data in Bundle and passed it to the appropriate Activity, Fragments can also receive and unpack these Bundle packages in the same way as implied by the signature of the onCreate method, as shown:

```
public void onCreate(Bundle savedInstanceState)
```

We can now put all our new information about Fragment, interfaces, singleton and a new trick, configuration qualifiers all together in our next app to create a true dual fragment app that behaves differently depending upon whether the device is in landscape or portrait orientation.

In this next app, we will not save data to a disk or add new data via the UI, as this could be almost copy pasted from the Note To Self app with minor amendments to cater for names and addresses instead of notes.