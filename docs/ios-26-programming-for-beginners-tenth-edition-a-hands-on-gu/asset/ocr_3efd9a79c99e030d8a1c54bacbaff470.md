```
func plateAndServe() -> String {
    return "Plating and serving done"
}
@IBAction func testButton(_ sender: UIButton) {
    print("Button tapped")
}
```

As you can see, this code simulates the process of making breakfast that was described in the previous section. Let’s break it down:

```
@IBOutlet var toastLabel: UILabel!
@IBOutlet var eggLabel: UILabel!
@IBOutlet var plateAndServeLabel: UILabel!
@IBOutlet var elapsedTimeLabel: UILabel!
```

These outlets are linked to four labels in the Main storyboard file. When you run the app, these labels will display the status of the toast and eggs, plating, and serving, as well as the time taken to complete the process.

```
override func viewDidAppear(_ animated: Bool) {
```

This statement method is called when the view controller’s view appears onscreen.

```
let startTime = Date().timeIntervalSince1970
```

This statement sets startTime to the current time, so the app can later calculate how long it takes to make the meal.

```
toastLabel.text = "Making toast..."
```

This statement makes toastLabel display the text Making toast....

```
toastLabel.text = makeToast()
```

This statement calls the makeToast() method, which waits for two seconds to simulate the time taken to make toast, and then returns the text Toast done, which will be displayed by toastLabel.

```
eggLabel.text = "Boiling eggs..."
```

This statement makes eggLabel display the text Boiling eggs....

```
eggLabel.text = boilEggs()
```