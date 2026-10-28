![](bbox=[0.28, 0.09, 0.77, 0.46])

Figure 17.21: Simulator showing the Journal Entry Detail screen

You have connected the Journal Entry Detail screen to the Map screen and have successfully passed data from a selected journal entry on the Map screen to the Journal Entry Detail screen. Fantastic! In the next section, you’ll configure the Journal Entry Detail screen to display a map snapshot of the journal entry’s location.

# Display a map snapshot on the Journal Entry Detail screen

The Map screen currently displays a single pin representing a journal entry. When you tap the pin on the Map screen and tap the callout button, the details of the journal entry are displayed on the Journal Entry Detail screen, but the second image view on the Journal Entry Detail screen currently displays a placeholder map image. You can capture a map region and convert it into an image using the MKMapSnapshotter class.