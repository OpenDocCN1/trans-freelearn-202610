You will now modify the MapViewController class to get an array of JournalEntry instances from the createSampleJournalEntryData() method and add it to the map view. Follow these steps:

+   1. In the Project navigator, click on the JournalEntry file. In the createSampleJournalEntryData() method, modify the statement that creates the journalEntry2 instance, as shown:

```
guard let journalEntry2 = JournalEntry(rating: 0, title: "Bad", body: "Today is a bad day", photo: photo2, latitude: 37.3318, longitude: -122.0312) else {
    fatalError("Unable to instantiate journalEntry2")
}
```

This instance now has values for its latitude and longitude properties, which will be used to set its coordinate property. The values used are for a location close to the Apple campus, which you will set in Simulator when you run the app.

> You can use whatever location you wish, but you will need to make sure that the location is close to the center point of the map; otherwise, the pin will not be displayed.

+   2. In the Project navigator, click on the MapViewController file. Add an extension after all other code in the file to make the MapViewController class conform to the MKMapViewDelegate protocol:

```
extension MapViewController: MKMapViewDelegate {
}
```

+   3. Just after the locationTask property declaration, add the following code to create a private property, annotations, that will hold an array of JournalItem instances:

```
private var locationTask: Task<Void, Error>?
private var annotations: [JournalEntry] = []
```

> There is currently no connection between the Journal List and Map screens. This means that any journal entries that you add using the Add New Journal Entry screen will not appear on the Map screen. You will create a shared instance that will be used by both view controllers in the next chapter.