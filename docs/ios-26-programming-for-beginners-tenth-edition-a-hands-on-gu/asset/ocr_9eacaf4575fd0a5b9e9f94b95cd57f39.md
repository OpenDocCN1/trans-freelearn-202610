Now you must determine when to call this method. View controllers have a view property. The view assigned to the view property will be automatically loaded when the view controller is loaded. After a view has been loaded successfully, the view controller’s viewDidLoad() method will be called. You will override the viewDidLoad() method in your TableViewControllerExample class to call createTableView(). Type in the following code just before the createTableView() method:

```
override func viewDidLoad() {
    super.viewDidLoad()
    view.bounds = CGRect(x: 0, y: -150, width: 375, height: 667)
    createTableView()
}
```

This sets the size of the live view, creates a table view instance, assigns it to tableView, and adds it as a subview to the view of the TableViewExampleController instance. The table view then calls the data source methods to determine how many table view cells to display and what to display in each cell. tableView(_:numberOfRowsInSection:) returns the number of elements in journalEntries, so three table view cells will be displayed. tableView(_:cellForRowAt:) creates the cell, creates a new cell configuration, sets the properties of the cell configuration, and assigns the updated configuration to the cell.

Verify that your completed playground looks like this:

```
import UIKit
import PlaygroundSupport
class TableViewExampleController: UIViewController, UITableViewDataSource, UITableViewDelegate {
    var tableView: UITableView!
    var journalEntries: [[String]] = [
        ["sun.max","12 Sept 2025","Nice weather today"],
        ["cloud.rain","13 Sept 2025","Heavy rain today"],
        ["cloud.sun","14 Sept 2025","It's cloudy out"]
    ]
    override func viewDidLoad() {
        super.viewDidLoad()
        view.bounds = CGRect(x: 0, y: -150, width: 375, height: 667)
        createTableView()
    }
    func createTableView() {
        tableView = UITableView(frame: CGRect(x: 0, y: 0,
```