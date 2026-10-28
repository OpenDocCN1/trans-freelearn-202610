
3. You’ll modify the class declaration as this class is now used to manage a collection view cell. Change the superclass to UICollectionViewCell:

```
class JournalListCollectionViewCell: UICollectionViewCell {
```

4. You’ll now assign this class as the identity of the collection view cell. Click the Main storyboard file in the Project navigator and click Collection View Cell under Journal Scene in the document outline:

![](img/0972ba626734a3986e095c788777818e_491_0.png)

Figure 22.25: Editor area showing Collection View Cell inside Journal Scene

5. Click the Identity inspector button. Under the Custom Class section, set Class to JournalListCollectionViewCell. This sets a JournalListCollectionViewCell instance as the custom collection view subclass for the collection view cell. Press Return when done:

![](img/0972ba626734a3986e095c788777818e_491_1.png)

Figure 22.26: Identity inspector showing Class set to JournalListCollectionViewCell

```
return filteredTableData.count
} else {
    return SharedData.shared.numberOfJournalEntries()
}
}
func collectionView(_ collectionView: UICollectionView,
cellForItemAt indexPath: IndexPath) ->
UICollectionViewCell {
    let journalCell =
    collectionView.dequeueReusableCell(withReuseIdentifier:
    "journalCell", for: indexPath) as!
    JournalListCollectionViewCell
    let journalEntry: JournalEntry
    if search.isActive {
        journalEntry = filteredTableData[indexPath.item]
    } else {
        journalEntry = SharedData.shared.journalEntry(at:
        indexPath.item)
    }
    if let photoData = journalEntry.photoData {
        journalCell.photoImageView.image = UIImage(data:
        photoData)
    } else {
        journalCell.photoImageView.image = nil
    }
    journalCell.dateLabel.text =
    journalEntry.date.formatted(
        .dateTime.month().day().year()
    )
    journalCell.titleLabel.text = journalEntry.entryTitle
    return journalCell
}
```

As you can see, they are very similar to the table view data source methods that you used previously.

9. As you are now using a collection view, tableView(_:commit:forRowAt:) can no longer be used to remove cells. Replace the tableView(_:commit:forRowAt:) method with the following method:

```
// MARK: - UICollectionView delete method
func collectionView(_ collectionView: UICollectionView,
contextMenuConfigurationForItemsAt indexPaths: [IndexPath], point: CGPoint) -> UIContextMenuConfiguration? {
    guard let indexPath = indexPaths.first else {
        return nil
    }
    let config = UIContextMenuConfiguration(previewProvider: nil) { elements -> UIMenu? in
        let delete = UIAction(title: "Delete") {
            action in
            if self.search.isActive {
                let selectedJournalEntry =
                self.filteredTableData[indexPath.item]
                self.filteredTableData.remove(at: indexPath.item)
                SharedData.shared
                .removeSelectedJournalEntry(selectedJournalEntry)
            } else {
                SharedData.shared.removeJournalEntry(at:
                indexPath.item)
            }
            collectionView.reloadData()
        }
        return UIMenu(children: [delete])
    }
    return config
}
```

Instead of swiping left to delete, this method implements a contextual menu with a single option, Delete, that appears when you tap and hold on a collection view cell.

10. You’ll also see an error in the prepare(for:sender:) method. Modify the guard statement in the prepare(for:sender:) method as shown:

```
guard let journalEntryDetailViewController =
segue.destination as?
JournalEntryDetailViewController, let indexPath =
collectionView.indexPathsForSelectedItems?.first
else {
    fatalError("Could not get indexpath")
}
```

All the errors in JournalListViewController have been resolved. Cool! In the next section, you’ll add code to change the size of the collection view cells based on device screen size and orientation.

## Dynamically modify collection view cell size using size classes

As you saw earlier, the table view on the Journal List screen presents table view cells using rows arranged in a single column. This works great on an iPhone, but as you have seen, this results in a lot of wasted space if you run the app on an iPad. Even though you can use the same UI for both iPhone and iPad, it would be better if you could customize it to suit each device.

To do this, you’ll add some code so your app can identify the size of the screen it’s running on, and you’ll dynamically modify the size of the collection view cells in the collection view to suit. You can identify the current screen size using size classes; you’ll learn about them in the next section.

## Size classes

To determine the size of the screen your app is running on, you must consider the effects of device orientation on your UI. It can be challenging to do this as there is a wide variety of screen sizes, in both portrait and landscape orientations. To make this easier, instead of using the physical resolution of the device, you will use size classes.

> For more information on size classes, see this link: https://developer.apple.com/design/human-interface-guidelines/layout.

Size classes are traits that are automatically assigned to a view by the operating system. Two classes are defined, which describe the height and width of a view: regular (expansive space) and compact (constrained space). Let’s look at size classes for a full-screen view on different devices:

| Device | Portrait | Landscape |
|---|---|---|
| iPad | Regular width<br>Regular height | Regular width<br>Regular height |
| iPhone 17 Pro Max | Compact width<br>Regular height | Regular width<br>Compact height |
| iPhone 16e | Compact width<br>Regular height | Compact width<br>Compact height |

Figure 22.33: Size classes for different iOS devices

For the JRNL app, you will configure the collection view in the Journal List screen to use a single column of collection view cells if the size class is compact, and two columns if the size class is regular.

You’ll add code to your app to determine the current size class. Once you know the size class, you’ll be able to set the number of columns to use and the size of the collection view cells in the collection view. You’ll learn how to do this in the next section.

## Modify the JournalListViewController class

You have already made the JournalListViewController class adopt the UICollectionViewDelegateFlowLayout protocol. Now you will create and set the collection view’s layout using a UICollectionViewFlowLayout instance and implement methods to dynamically set the collection view cell size.

Follow these steps:

- 1. Click the JournalListViewController file in the Project navigator. In the JournalListViewController class, add the following method to the class definition before the closing curly brace:

```
func setupCollectionView() {
    let flowLayout = UICollectionViewFlowLayout()
    flowLayout.sectionInset = UIEdgeInsets(top: 10, left: 10,
```

2. Choose Rotate Left from the Device menu, and you’ll still see two columns, but the cells have expanded in size to fill the screen:

![](img/0972ba626734a3986e095c788777818e_503_0.png)

Figure 22.35: Simulator showing iPad screen rotated to the left with two columns

3. Stop your app and choose iPhone 16e from the Destination menu. Run your app on Simulator again, and it will display a single column, as shown:

![](img/0972ba626734a3986e095c788777818e_503_1.png)

Figure 22.36: Simulator showing iPhone screen with a single column

Simulator will not automatically close the iPad instance when it launches the iPhone instance. Close the iPad instance manually for better performance.
