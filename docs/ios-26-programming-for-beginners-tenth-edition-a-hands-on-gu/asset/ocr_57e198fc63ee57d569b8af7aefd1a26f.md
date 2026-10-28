You’ve now successfully connected the UI elements in **Entry Detail Scene** to the outlets in the JournalEntryDetailViewController class. In the next section, you will implement the code to display the details of a JournalEntry instance when the user taps a table view cell on the Journal List screen.

# Display the details of a journal entry

Until this point, you have implemented the JournalEntryDetailViewController class and connected the outlets in this class to the labels, text view, and image view in **Entry Detail Scene**. When the user taps a table view cell on the Journal List screen, you’ll get the corresponding JournalEntry instance from the data source and pass it to the JournalEntryDetailViewController instance to display on the Journal Entry Detail screen. To do this, you will implement the prepare(for:sender:) method in the JournalListViewController class. Follow these steps:

+   1. In the Project navigator, click the **Main** storyboard file and click the segue connecting the **Journal** scene and the **Entry Detail** scene. Click the Attributes inspector button and, under **Storyboard Segue**, set **Identifier** to entryDetail:

![](bbox=[0.19, 0.46, 0.865, 0.655])

Figure 16.21: Attributes inspector showing Identifier set to entryDetail

You will use this identifier later to identify the segue used to go from the Journal List screen to the Journal Entry Detail screen.