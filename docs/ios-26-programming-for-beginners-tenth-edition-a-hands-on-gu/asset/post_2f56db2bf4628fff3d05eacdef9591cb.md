
To see how MVC works in the context of an iOS app, let’s learn more about view controllers. You will see what it takes to implement a view controller that is required to manage a table view, which is used on the Journal List screen.

## View controllers

So far, you have implemented JournalListViewController, a view controller that manages the table view on the Journal List screen. However, you still haven’t learned how the code you added to it works, so let’s look at that now.

You may wish to re-read Chapter 11, Build the User Interface, where you implemented the JournalListViewController class.

When an iOS app is launched, the view controller for the first screen to be displayed is loaded. The view controller has a view property and automatically loads the view instance assigned to its view property. That view may have subviews, which are also loaded.

If one of the subviews is a table view, it will have dataSource and delegate properties. The dataSource property is assigned to an object that provides data to the table view. The delegate property is assigned to an object that handles user interaction with the table view. Typically, the view controller for the table view will be assigned to the table view’s dataSource and delegate properties.

The method calls that a table view will send to its view controller are declared in the UITableViewDataSource and UITableViewDelegate protocols. Remember that protocols only provide method declarations; the implementation of those method calls is in the view controller. The view controller will then get the data from the model objects and provide it for the table view. The view controller also handles user input and modifies the model objects as required.

Let’s take a closer look at table views and table view protocols in the next section.

5. In a class definition, convention dictates that properties are declared at the top before any method declarations. Rearrange the code so that the property declarations are at the top, as follows:

```
class TableViewExampleController: UIViewController,
UITableViewDataSource {
    var tableView: UITableView!
    var journalEntries: [[String]] = [
        ["sun.max","12 Sept 2025","Nice weather today"],
        ["cloud.rain","13 Sept 2025","Heavy rain today"],
        ["cloud.sun","14 Sept 2025","It's cloudy out"]
    ]
    func tableView(_ tableView: UITableView,
    numberOfRowsInSection section: Int) -> Int {
```

6. To make the table view display a row for each element inside the journalEntries array, click the code placeholder inside the tableView(_:numberOfRowsInSection:) method definition and type in journalEntries.count. The completed method should look like this:

```
func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    journalEntries.count
}
```

journalEntries.count returns the number of elements inside the journalEntries array. Since there are three elements in it, this will make the table view display three rows.

7. To make the table view display journal entry details in each cell, click the code placeholder inside the tableView(_:cellForRowAt:) method definition and type the following:

```
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath)
-> UITableViewCell {
    let cell = tableView.dequeueReusableCell(withIdentifier:
    "cell", for: indexPath)
    let journalEntry = journalEntries[indexPath.row]
    var content = cell.defaultContentConfiguration()
    content.image = UIImage(systemName: journalEntry[0])
    content.text = journalEntry[1]
```

```
content.secondaryText = journalEntry[2]
cell.contentConfiguration = content
return cell
}
```

Let’s break this down:

```
let cell = tableView.dequeueReusableCell(withIdentifier: "cell",
for: indexPath)
```

This statement creates a new table view cell or reuses an existing table view cell and assigns it to cell. Imagine you have 1,000 items to display in a table view. You don’t need 1,000 rows containing 1,000 table view cells—you only need just enough to fill the screen. Table view cells that scroll off the top of the screen can be reused to display items that appear at the bottom of the screen, and vice versa. As table views can display more than one type of cell, you set the cell reuse identifier to "cell" to identify this particular table view cell type. This identifier will be registered with the table view later.

```
let journalEntry = journalEntries[indexPath.row]
```

The indexPath value locates the row in the table view. The first row has indexPath containing section 0 and row 0. indexPath.row returns 0 for the first row, so this statement assigns the first element in the journalEntries array to journalEntry.

```
var content = cell.defaultContentConfiguration()
```

By default, a UITableViewCell instance can store an image, a text string, and a secondary text string. You set these by using the table view cell’s content configuration property. This statement retrieves the default content configuration for the table view cell’s style and assigns it to a variable, content.

```
content.image = UIImage(systemName: journalEntry[0])
content.text = journalEntry[1]
content.secondaryText = journalEntry[2]
cell.contentConfiguration = content
```

These statements update content with details from journalEntry, which is an array that has three elements. The first element is used to specify an image that is assigned to the image property. The second element is assigned to the text property. The third element is assigned to the secondaryText property. The last line assigns content to the table view cell’s contentConfiguration property.

```
return cell
```

This statement returns the table view cell, which is then displayed on the screen.

The tableView(_:cellForRowAt:) method is executed for each row in the table view.

8. To handle table view cell deletion, type the following code after the implementation of tableView(_:cellForRowAt):

```
func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCell.EditingStyle, forRowAt indexPath: IndexPath) {
    if editingStyle == .delete {
        journalEntries.remove(at: indexPath.row)
        tableView.reloadData()
    }
}
```

This removes the journalEntries element corresponding to the table view cell that the user swiped left on and reloads the table view.

The TableViewExampleController class now conforms to the UITableViewDataSource protocol. In the next section, you will make it conform to the UITableViewDelegate protocol.

## Adopt the UITableViewDelegate protocol

A user can tap on a table view cell to select it. To handle user interaction, you will make the TableViewExampleController class adopt the UITableViewDelegate protocol. You will implement one optional method from this protocol, tableView(_:didSelectRowAt:), which is called by the table view when the user taps a row. Follow these steps:

+   1. To make the TableViewExampleController class adopt the UITableViewDelegate protocol, type a comma after UITableViewDataSource in the class declaration, and then type UITableViewDelegate. Your code should look like this:

```
class TableViewExampleController: UIViewController,
UITableViewDataSource, UITableViewDelegate {
```

2. Type the following after the implementation of tableView(_:commit:forRowAt:):

```
func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
    let selectedJournalEntry = journalEntries[indexPath.row]
    print(selectedJournalEntry)
}
```

This method will get the journalEntries array element corresponding to the tapped row and print it to the Debug area.

3. Verify that your TableViewExampleController class looks like this:

```
class TableViewExampleController: UIViewController, UITableViewDataSource, UITableViewDelegate {
    var tableView: UITableView!
    var journalEntries: [[String]] = [
        ["sun.max","12 Sept 2025","Nice weather today"],
        ["cloud.rain","13 Sept 2025","Heavy rain today"],
        ["cloud.sun","14 Sept 2025","It's cloudy out"]
    ]
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        journalEntries.count
    }
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "cell", for: indexPath)
        let journalEntry = journalEntries[indexPath.row]
        var content = cell.defaultContentConfiguration()
        content.image = UIImage(systemName: journalEntry[0])
        content.text = journalEntry[1]
        content.secondaryText = journalEntry[2]
        cell.contentConfiguration = content
        return cell
    }
    func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCell.EditingStyle,
```
