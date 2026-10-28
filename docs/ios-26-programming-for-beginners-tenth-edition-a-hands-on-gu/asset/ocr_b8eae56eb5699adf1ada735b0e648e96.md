# Modify JournalEntryDetailViewController to display selected journal entry details

When transitioning to the Journal Entry Detail screen, the JournalEntry instance corresponding to the table view cell that the user tapped will be assigned to the JournalEntryDetailViewController instance's selectedJournalEntry property. You will add code to the JournalEntry DetailViewController class to access this property and display the details for a journal entry. Follow these steps:

+   1. In the Project navigator, click the JournalEntryDetailViewController file and modify the JournalEntryDetailViewController class's viewDidLoad() method as shown to display the details of the journal entry:

```
override func viewDidLoad() {
    super.viewDidLoad()
    dateLabel.text = selectedJournalEntry?.date.formatted(
        .dateTime.day().month(.wide).year()
    )
    titleLabel.text = selectedJournalEntry?.title
    bodyTextView.text = selectedJournalEntry?.body
    photoImageView.image = selectedJournalEntry?.photo
}
```

As you can see, the properties of the JournalEntry instance that was passed to this view controller earlier are used to populate the user interface elements. Note that the date property needs to be formatted into a string before it can be assigned to the dateLabel.text property.

+   2. Build and run your project and tap a table view cell. The details of the journal entry corresponding to that table view cell will be displayed on the Journal Entry Detail screen: