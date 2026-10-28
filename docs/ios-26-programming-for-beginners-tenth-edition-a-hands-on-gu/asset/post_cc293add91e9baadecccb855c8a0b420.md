
16. Connect the locationSwitchValueChanged action to the Get Location switch, and choose Value Changed from the pop-up menu:

![](img/0972ba626734a3986e095c788777818e_377_0.png)

Figure 17.4: Attributes inspector showing the getLocationSwitchValueChanged outlet

You have completed modifying the AddJournalEntryViewController class. In the next section, you'll learn how to configure your app to access user data.

## Modify the Info.plist file

Since your app uses user data, you will need to ask the user for permission to use it. To do so, you will add a new setting to your app’s Info property list file. Follow these steps:

+   1. In the Project navigator, click on the Info property list file. If you move your pointer over the Information Property List row, you’ll see a small + button. Click it to create a new row:

![](img/0972ba626734a3986e095c788777818e_378_0.png)

Figure 17.5: Editor area showing contents of the Info property list file

+   2. In the new row, set Key to Privacy - Location When In Use Usage Description and Value to This app uses your location for journal entries.. Your Info property list file should look like the following when you’re done:

![](img/0972ba626734a3986e095c788777818e_378_1.png)

Figure 17.6: Info property list file with a new row added

+   3. Launch Simulator and choose Location | Apple from Simulator’s Features menu to simulate a location:

## Create the MapViewController class

In Chapter 12, Complete the User Interface, you added a map view to the Map screen. A map view is an instance of the MKMapView class. You can see what it looks like in the Apple Maps app.

To learn more about MKMapView, see https://developer.apple.com/documentation/mapkit/mkmapview.

When you build and run your app, you will see a map on the screen. The part of the map that is visible onscreen can be specified by setting the map view’s region property.

To learn more about regions and how to make them, see https://developer.apple.com/documentation/mapkit/mkmapview/1452709-region.

You’ll create a new class, MapViewController, to be the view controller for the Map screen, and you’ll use Core Location to determine the center point of the map region that will be displayed. Follow these steps:

+   1. Create a new group inside your project by right-clicking the JRNL group and choosing New Group. Name this group Map Screen and move it so it is below the Journal Entry Detail Screen group.
+   2. Right-click on the Map Screen group and select New File From Template....
+   3. iOS should already be selected. Choose Cocoa Touch Class and click Next.
+   4. Configure the class with the following details:
+      - Class: MapViewController
+      - Subclass: UIViewController
+      - Also create XIB: Unchecked
+      - Language: Swift
+      Click Next when you’re done.
+   5. Click Create. The MapViewController file appears in the Project navigator, and its contents appear in the Editor area.

```
} else if update.locationUnavailable {
    failedToGetLocation(message: "Location Unavailable")
}
}
}
}
```

This method is like the fetchUserLocation() method you implemented in the AddJournalEntryViewController extension. First, it will ask for permission to use private user data and set the title of the Map screen to Getting location. . . . Next, an asynchronous task to continuously determine the user’s location will be assigned to locationTask, and if the user’s location is found, the map will be updated to show the user’s location. Otherwise, an alert with an appropriate error message will be displayed.

Note that since the methods called by fetchUserLocation() have not been implemented yet, you will see error messages.

+   11. Implement the missing methods in the MapViewController extension before the closing curly brace:

```
private func updateMapWithLocation(_ location: CLLocation) {
    let interval = location.timestamp.timeIntervalSinceNow
    if abs(interval) < 30 {
        locationTask?.cancel()
        let lat = location.coordinate.latitude
        let long = location.coordinate.longitude
        navigationItem.title = "Map"
        mapView.region = MKCoordinateRegion(center:
        CLLocationCoordinate2D(latitude: lat,
        longitude: long), span: MKCoordinateSpan(
        latitudeDelta: 0.01, longitudeDelta: 0.01))
    }
}
private func failedToGetLocation(message: String) {
    locationTask?.cancel()
    navigationItem.title = "Location not found"
    let alertController = UIAlertController(title:
    "Failed to get location", message: message,
    preferredStyle: .alert)
```

```
let okAction = UIAlertAction(title: "OK",
style: .default)
alertController.addAction(okAction)
present(alertController, animated: true)
}
```

These methods are like the updateCurrentLocation(location:) and failedToGetLocation(message:) methods you implemented earlier in the AddJournalEntryViewController extension.

The updateMapWithLocation(location:) method will set the title of the Map screen to Map, create a map region centered on the user’s location, and assign it as the region for the mapView property.

The failedToGetLocation(message:) method will set the title of the Map screen to Location not found and display an appropriate error message if permission to use user data was denied or if the location is unavailable.

+   12. Modify the viewDidLoad() method for the MapViewController class to call fetchUserLocation() as shown:

```
override func viewDidLoad() {
    super.viewDidLoad()
    fetchUserLocation()
}
```

+   13. Click on the Main storyboard file in the Project navigator and then click on the first Map Scene in the document outline. Click on the Identity inspector button, and under Custom Class, set Class to MapViewController:

![](img/0972ba626734a3986e095c788777818e_384_0.png)

Figure 17.10: Identity inspector settings for Map scene

14. Click on the Connections inspector to display all the outlets for Map Scene. Drag from the mapView outlet to the map view in Map Scene:

![](img/0972ba626734a3986e095c788777818e_385_0.png)

Figure 17.11: Connections inspector showing the mapView outlet

Remember that if you make a mistake, you can click on the x to break the connection and drag from the outlet to the UI element once more.

15. Build and run your app and verify that Apple is selected from Simulator’s Features | Location menu. Tap the Map tab button to display a map region centered on the location you selected, which, in this case, is the Apple campus:

![](img/0972ba626734a3986e095c788777818e_386_0.png)

Figure 17.12: Simulator showing a map centered on the location you selected

Since viewDidLoad() is only called once when the MapViewController instance loads its view, the map will not be updated if the user’s location changes after it was initially set. Also, you’ll notice that it takes a long time for the location to be determined if you’re running the app on an actual iOS device. We’ll address both issues in the next chapter.

You can simulate any location in Simulator by choosing Features | Location | Custom Location and entering the longitude and latitude of the desired location.

You have successfully created a new view controller for the Map screen and configured it to display a map region centered on your device location. Excellent! In the next section, you’ll learn about the MKAnnotation protocol and how to make a class conform to it.

## Update the JournalEntry class to conform to the MKAnnotation protocol

When you use the Maps app on iPhone, you can tap and hold on the map to drop a pin:

![](img/0972ba626734a3986e095c788777818e_387_0.png)

2. The MKAnnotation protocol has an optional title property, which you will use later. You will change the name of the title property in the JournalEntry class to entryTitle so you don’t have two properties with the same name. Right-click on the title property and choose Refactor | Rename from the pop-up menu.

3. Set the new name to entryTitle, as shown, and click Rename:

![](img/0972ba626734a3986e095c788777818e_389_0.png)

Figure 17.14: Refactoring the title property to entryTitle

4. Modify the JournalEntry class declaration as follows to make it a subclass of the NSObject class and to adopt the MKAnnotation protocol:

```
class JournalEntry: NSObject, MKAnnotation {
```

5. You’ll see an error because you have not yet implemented the coordinate property, which is required to conform to the MKAnnotation protocol. Type the following after the initializer:

```
// MARK: - MKAnnotation
var coordinate: CLLocationCoordinate2D {
    guard let latitude, let longitude else {
        return CLLocationCoordinate2D()
    }
}
```
