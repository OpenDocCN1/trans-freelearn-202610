This method attempts to use a JSONEncoder instance to encode the journalEntries array to JSON format and subsequently write it to the file stored at the location specified by savedDataURL.

You have implemented the methods to load and save journal entries in your app. Now you’ll modify the SharedData class to call these methods at appropriate times. Follow these steps:

+   1. In the SharedData class, modify the initializer to call the loadJournalEntriesData() method:

```
// MARK: - Initialization
private init() {
    loadJournalEntriesData()
}
```

This will load any saved journal entries as the app is starting up.

+   2. Modify the addJournalEntry(_:) method to call the saveJournalEntriesData() method after a new journal entry has been added:

```
func addJournalEntry(_ newJournalEntry: JournalEntry) {
    journalEntries.insert(newJournalEntry, at: 0)
    saveJournalEntriesData()
}
```

+   3. Modify the removeJournalEntry(at:) method to call saveJournalEntriesData() after a row has been removed from the table view:

```
func removeJournalEntry(at index: Int) {
    journalEntries.remove(at: index)
    saveJournalEntriesData()
}
```

+   4. Build and run your app. Verify a simulated location has been set and add a new journal entry. Your app should work the way it did before.