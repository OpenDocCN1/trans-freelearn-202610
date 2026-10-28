# Add a subtitle to the Journal List screen

In iOS 26, the UINavigationItem object for each view controller gets a new subtitle property that appears below the title. You'll add code to display the number of journal entries below the title on the Journal List screen. Follow these steps:

+   1. Click the **JournalListViewController** file in the Project navigator. Add a new method after the setupCollectionView() method to display the number of journal entries in the subtitle of the Journal List screen:
+   ```swift
+   func updateSubtitleCount() {
+       let noOfJournalEntries = SharedData.shared.numberOfJournalEntries
+       switch noOfJournalEntries {
+       case 0:
+           navigationItem.subtitle = "No journal entries"
+       case 1:
+           navigationItem.subtitle = "1 journal entry"
+       default:
+           navigationItem.subtitle = "\(noOfJournalEntries) journal entries"
+       }
+   }
+   ```
+   2. Call updateSubtitleCount() in viewDidLoad() before the closing curly brace. This will display the number of journal entries in the Journal List screen when the app is launched:
+   ```swift
+   navigationItem.searchController = search
+   updateSubtitleCount()
+   }
+   ```
+   3. The number of journal entries displayed needs to be updated when a journal entry is deleted. Modify collectionView(_:contextMenuConfigurationForItemsAt:point:) to call updateSubtitleCount() when a journal entry is deleted:
+   ```swift
+   collectionView.reloadData()
+   self.updateSubtitleCount()
+   }
+   return UIMenu(children: [delete])
+   ```