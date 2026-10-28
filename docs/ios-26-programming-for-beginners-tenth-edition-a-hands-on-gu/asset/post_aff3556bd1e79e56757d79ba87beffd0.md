
![](img/0972ba626734a3986e095c788777818e_453_0.png)

Figure 20.9: Simulator showing image picker

If you run the app on an actual iOS device, a dialog box will appear asking for permission to use the camera. Click **OK** to continue.

5. Select a photo and it will appear in the image view on the Add New Journal Entry screen. Enter sample details for the journal entry and click Save:

![](img/0972ba626734a3986e095c788777818e_454_0.png)

Figure 20.10: Simulator showing photo on the Add New Journal Entry screen

6. You will be returned to the Journal List screen. Tap the newly added journal entry. You will see that the photo is displayed on the Journal Entry Detail screen:

![](img/0972ba626734a3986e095c788777818e_455_0.png)

Figure 20.11: Simulator showing photo on the Journal Entry Detail screen

You can now add photos to a new journal entry on the Add New Journal Entry screen and display them on the Journal Entry Detail screen. Fantastic!

## Summary

In this chapter, you completed the implementation of the Add New Journal Entry screen by adding a way for the user to get a photo from the camera or photo library, which then can be added to a new journal entry. First, you added a tap gesture recognizer to the image view in the Add New Journal Entry screen and configured it to display an image picker controller. Then, you implemented the UIImagePickerDelegate protocol, which allows you to get a photo from the camera or the photo library, and made the photo smaller before it is saved to the journal entry instance. You also modified the Info property list file to allow you to access the camera and photo library.

You are now able to write your own apps that import photos from your camera or photo library.

In the next chapter, you'll implement a way for the user to search through the journal entries on the Journal List screen.

## Join us on Discord

Read this book alongside other users and the author himself.

Ask questions, provide solutions to other readers, chat with the author via Ask Me Anything sessions, and much more. Scan the QR code or visit the link to join the community.

https://packt.link/LdIDT/

![](img/0972ba626734a3986e095c788777818e_456_0.png)

This statement sets the JournalListViewController instance as the object responsible for updating the search results.

```
search.obscuresBackgroundDuringPresentation = false
```

This statement obscures the view controller containing the search content when the user interacts with the search bar. Since you’re using the table view on the Journal List screen to display the search results, this value is set to false, otherwise, you will obscure the search results.

```
search.searchBar.placeholder = "Search titles"
```

This statement sets the placeholder text for the search bar.

```
navigationItem.searchController = search
```

This statement adds the search bar to the navigation bar on the screen.

5. Build and run your app, and you’ll see a search bar on the Journal List screen. Type some text into the search bar:

![](img/0972ba626734a3986e095c788777818e_460_0.png)

Figure 21.1: Simulator showing a search bar on the Journal List screen

6. Note that the text you type into the search bar appears in the Debug area:

![](img/0972ba626734a3986e095c788777818e_461_0.png)

Figure 21.2: Debug area showing the search text

You’ve added a search bar to the Journal List screen. Great! In the next section, you’ll modify the JournalListViewController file to display journal entries whose titles match the search text typed into the search bar.

## Modify table view data source methods

As you learned in Chapter 14, MVC and Table Views, you can use UITableViewDataSource methods to determine how many table view rows to display, and what to put in each row.

In the previous section, you added a new property, filteredTableData, to hold an array of JournalEntry instances that match the search text. You’ll modify the updateSearchResults(for:) method to populate filteredTableData with JournalEntry instances that match the search text, and you’ll modify the UITableViewDataSource methods to display the contents of filteredTableData on the Journal List screen while the search bar is active. Follow these steps:

- 1. In the Project navigator, click the JournalListViewController file. Modify the updateSearchResults(for:) method in the UISearchResultsUpdating extension as shown:

```
//MARK: - Search
func updateSearchResults(for searchController: UISearchController) {
    guard let searchBarText = searchController.searchBar.text
    else { return }
    filteredTableData = SharedData.shared.allJournalEntries.filter {
        entry in
        entry.entryTitle.lowercased().contains(searchBarText.
```

4. Build and run your app, and type some text that matches the title of one of your journal entries into the search bar. The journal entries with titles that match the search text will be displayed:

![](img/0972ba626734a3986e095c788777818e_463_0.png)

Figure 21.3: Simulator showing journal entries with titles that match the search text

You can now display journal entries with titles that match the search text, but when you tap on them, the Journal Entry Detail screen may or may not display the details for the tapped journal entry. You will fix this issue in the next section.

## Modify the prepare(for:sender:) method

When you enter text in the search bar, the journal entries with titles matching the search text will appear in the table view on the Journal List screen. But if you were to tap one of them, the Journal Entry Detail screen may or may not display the details for the tapped journal entry. This is because the prepare(for:sender:) method will reference the journalEntries array in the SharedData.shared instance instead of the filteredTableData array. To fix this, follow these steps:

- 1. Modify the prepare(for:sender:) method in the JournalListViewController class as shown to assign the appropriate JournalEntry instance from the filteredTableData array to the journalEntry property for the destination view controller when the search bar is active:

```
guard let journalEntryDetailViewController = segue.destination as? JournalEntryDetailViewController, let indexPath = tableView.indexPathForSelectedRow else {
    fatalError("Could not get indexPath")
}
let selectedJournalEntry: JournalEntry
if search.isActive {
```

Swiping left on a table view row when the search bar is active will delete it from the table view and delete the corresponding journal entry from the journalEntries array in the SharedData instance.

6. Verify that the deleted journal entry no longer appears on the Journal List screen:

![](img/0972ba626734a3986e095c788777818e_469_0.png)

Figure 21.8: Simulator showing remaining journal entries on the Journal List screen

You have successfully modified the method to delete journal entries, and the implementation of a search bar on the Journal List screen is now complete. Excellent!

## Summary

In this chapter, you implemented a search bar for the Journal List screen. First, you modified the JournalListViewController class to conform to the UISearchResultsUpdating protocol and display a search bar on the Journal List screen. Next, you modified the data source methods to display the correct journal entries when the user types in a search term. After that, you modified the prepare(for:sender:) method to ensure that the correct journal entry details are displayed on the Journal Entry Detail screen. Finally, you modified the method used to remove journal entries.

You have now learned how to implement a search bar for your own apps, and you have also completed the JRNL app. Fantastic job!

In the next chapter, you’ll learn how to get your app ready for iPads and Macs.
