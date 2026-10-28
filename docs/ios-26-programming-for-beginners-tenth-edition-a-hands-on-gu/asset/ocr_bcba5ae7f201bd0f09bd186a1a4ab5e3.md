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