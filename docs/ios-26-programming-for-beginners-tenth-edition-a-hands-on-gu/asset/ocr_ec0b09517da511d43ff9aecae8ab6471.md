# 17

# Core Location and MapKit

In the previous chapter, you learned how to pass data from the Add New Journal Entry screen to the Journal List screen, and from the Journal List screen to the Journal Entry Detail screen. You also learned about the UITextFieldDelegate and UITextViewDelegate methods.

In this chapter, you’ll learn how to get your device location using Apple’s Core Location framework, and how to set map regions, display map annotations, and create map snapshots using Apple’s MapKit framework. This will come in handy if you’re planning to build apps that use maps, like Apple Maps or Waze.

First, you’ll modify the Add New Journal Entry screen so that the user can add their current location to a new journal entry. Next, you’ll create a MapViewController class (a view controller for the Map screen) and configure it to display a map region centered on your location. Then, you’ll update the JournalEntry class to conform to the MKAnnotation protocol, which lets you add journal entries as map annotations to a map. After that, you’ll modify the MapViewController class to display a pin for each journal entry within the map region that you set earlier. You’ll configure the pins to display callouts and configure buttons in the callouts to display the Journal Entry Detail screen when tapped. Finally, you’ll modify the JournalEntryViewController class to display a map snapshot showing the location where the journal entry was made on the Journal Entry Detail screen.

By the end of this chapter, you’ll have learned how to use Core Location to get your device location and how to use MapKit to specify a map region, add map annotation views to a map, and create map snapshots.