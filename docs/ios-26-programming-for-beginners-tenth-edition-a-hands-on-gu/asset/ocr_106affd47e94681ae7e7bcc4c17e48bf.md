2. Add an extension for the AddJournalEntryViewController class after all other code in the file:

```
extension AddJournalEntryViewController {
    // MARK: - CoreLocation
}
```

You’ll implement code to ask for permission to use the user’s private data and to determine the user’s location in this extension later.

3. Add outlets for the Get Location switch and the label next to it after all the other outlets in the AddJournalEntryViewController class:

```
@IBOutlet var photoImageView: UIImageView!
@IBOutlet var saveButton: UIBarButtonItem!
@IBOutlet var getLocationSwitch: UISwitch!
@IBOutlet var getLocationSwitchLabel: UILabel!
var newJournalEntry: JournalEntry?
```

4. Add properties to store an instance of the CLLocationManager class, an asynchronous task that will manage location updates, and the current device location after all other property declarations:

```
var newJournalEntry: JournalEntry?
private let locationManager = CLLocationManager()
private var locationTask: Task<Void, Error>?
private var currentLocation: CLLocation?
```

All these properties are private as they will only be used within this class.

5. Implement a method to determine the user’s location in the AddJournalEntryViewController extension before the closing curly brace:

```
private func fetchUserLocation() {
    locationManager.requestWhenInUseAuthorization()
    locationTask = Task {
        for try await update in
        CLLocationUpdate.liveUpdates() {
            if let location = update.location {
                updateCurrentLocation(location)
            } else if update.authorizationDenied {
```