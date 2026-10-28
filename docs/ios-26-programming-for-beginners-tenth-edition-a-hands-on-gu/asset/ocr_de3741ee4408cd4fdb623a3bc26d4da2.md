Remember that if you make a mistake, you can click the x button to break the connection and drag from the outlet to the UI element once more.

The journalCell table view cell in the Main storyboard file has now been set up with a custom table view subclass, JournalListTableViewCell. The outlets for the table view cell’s image view and labels have also been created and assigned. Now, you will be able to set the photoImageView, dateLabel, and titleLabel outlets to display a photo, a date, and a title in each cell when the app is run.

In the next section, you’ll update the table view data source methods in the JournalListViewController class to provide the number of table view cells to be displayed in the table view, as well as to provide the journal entry photo, date, and title for each cell.

# Update the data source methods in JournalListViewController

The data source methods in the JournalListViewController class are currently set to display 10 table view cells, with each cell containing an image view displaying a smiley face and two labels. You’ll update them to get the number of cells to display and the data to put in each cell, from the SampleJournalEntryData instance. Follow these steps:

+   1. Click the JournalListViewController file in the Project navigator.

+   2. Rearrange the code in the JournalListViewController class so that the tableView outlet and the viewDidLoad() method are located before the table view delegate methods:

```
class JournalListViewController: UIViewController,
UITableViewDataSource, UITableViewDelegate {
@IBOutlet var tableView: UITableView!
override func viewDidLoad() {
    super.viewDidLoad()
}
func tableView(_ tableView: UITableView,
numberOfRowsInSection section: Int) -> Int {
```