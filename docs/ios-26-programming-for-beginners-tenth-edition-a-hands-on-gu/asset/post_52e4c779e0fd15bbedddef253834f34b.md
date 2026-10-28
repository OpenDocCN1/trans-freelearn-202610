
Check out the following video to see the code in action:

https://youtu.be/J0ueVai569s

Let’s start by learning how to create a custom UIStackView subclass that will display a star rating on the screen.

## Create a custom UIStackView subclass

You’ve only used Apple’s predefined UI elements so far, such as labels and buttons. All you had to do was click the Library button, search for the object you want, and drag it into the storyboard. However, there will be cases where the objects provided by Apple are either unsuitable or don’t exist. In such cases, you will need to build your own. Let’s review the Add New Journal Entry screen that you saw in the app tour:

![](img/0972ba626734a3986e095c788777818e_428_0.png)

Figure 19.1: Add New Journal Entry screen showing the star rating

You can see a group of five stars just above the Get Location switch. Currently, the New Entry scene and the Entry Detail scene in the Main storyboard file have placeholder view objects where the stars should be. You will create the RatingView class, a custom subclass of the UIStackView class, which you will use in both scenes. An instance of this class will display ratings as stars.

For the rest of this chapter, an instance of the RatingView class will be referred to as a rating view (the same way an instance of the UIButton class is referred to as a button).

Let’s begin by creating a subclass of the UIStackView class. Follow these steps:

- 1. In the Project navigator, right-click on the JRNL folder and choose New Group from the pop-up menu. Name this group View. Move this group to a new location below the Model group:

9. Call the setupButtons() method in the initializer:

```
required init(coder: NSCoder) {
    super.init(coder: coder)
    setupButtons()
}
```

This draws the rating view onscreen when the rating view is initialized.

You have created a custom UIStackView subclass named RatingView, and you have added code to make it draw five stars on the screen. Now let’s add code to enable the user to change the rating when the stars in the rating view are tapped. Follow these steps:

1. Implement a method to change the rating view’s rating property when a button in the ratingButtons array is tapped after the setupButtons() method:

```
@objc private func ratingButtonTapped(_ button: UIButton) {
    guard let index = ratingButtons.firstIndex(of: button) else {
        fatalError("The button, \(button), is not in the ratingButtons array: \(ratingButtons)")
    }
    let selectedRating = index + 1
    if selectedRating == rating {
        rating = 0
    } else {
        rating = selectedRating
    }
}
```

When a button in the ratingButtons array is tapped, the guard statement assigns the index of the button to index. selectedRating is then set to the value stored in index + 1. If the rating property has the same value as selectedRating, it is set to 0; otherwise, it is set to the same value as selectedRating.

For example, let’s say you tap the third star in the rating view. Since the third star is the third element in the ratingButtons array, index would be set to 2 and selectedRating would be set to 2 + 1 = 3. Assuming that the initial value of the rating property is 0, selectedRating == rating would return false, and the rating property’s value would be set to 3.

2. Assign this method as the button action in the for loop of the setupButtons method after the statements setting the constraints:

```
button.widthAnchor.constraint(equalToConstant: starSize.width).
isActive = true
button.addTarget(self, action: #selector(RatingView.
ratingButtonTapped(_:)), for: .touchUpInside)
addArrangedSubview(button)
```

3. Add a method to change the button state according to the rating that was set before the closing curly brace:

```
private func updateButtonSelectionStates() {
    for (index, button) in ratingButtons.enumerated() {
        button.isSelected = index < rating
    }
}
```

To see how this works, let’s say that the rating property is set to 3. The default state for each button is .normal.

The first button is at index 0, so button.isSelected is 0 < 3, which returns true. Since the image for the .selected state is a filled star, this button’s image is set to a filled star. The same is true for the next two buttons.

The fourth button is at index 3, so button.isSelected is 3 < 3, which returns false. This means that the button’s state remains .normal. The image for the .normal state is a star outline, so this button’s image is set to a star outline. The same is true for the fifth button.

In short, when the rating property is set to 3, the rating view displays the first three buttons with filled stars, and the remaining two buttons with star outlines.

4. You’ll need to call the updateButtonSelectionStates() method every time the rating property’s value changes. To do so, modify the rating property as shown:

```
var rating = 0 {
    didSet {
        updateButtonSelectionStates()
    }
}
```

This is known as a property observer, and every time the rating property’s value changes, the updateButtonSelectionStates() method will be called.

You have completed the implementation of the rating view. In the next section, you’ll add it to the Add New Journal Entry screen.

## Add your custom view to the Add New Journal Entry screen

So far, you have created a new RatingView class in your project and configured it to set its rating property when a star in it is tapped. In this section, you will set the identity of the stack view object above the Get Location switch in the New Entry scene to the RatingView class, configure an outlet for it in the AddJournalEntryViewController class, and add code to use the rating property’s value when creating a new journal entry. Follow these steps:

- 1. In the Project navigator, click the AddJournalEntryViewController file. Add a new outlet for a rating view in the AddJournalEntryViewController class after all other property declarations:

```
@IBOutlet var getLocationSwitch: UISwitch!
@IBOutlet var getLocationSwitchLabel: UILabel!
@IBOutlet var ratingView: RatingView!
```

- 2. Modify the prepare(for:sender:) method to get the rating view’s rating property value when creating a new journal entry:

```
let photo = photoImageView.image
let rating = ratingView.rating
let lat = currentLocation?.coordinate.latitude
```

- 3. Click the Main storyboard file and select New Entry Scene in the document outline. Click the UIStackView object above the Get Location switch, as shown:

![](img/0972ba626734a3986e095c788777818e_435_0.png)

Figure 19.3: Editor area showing UIStackView object above the Get Location switch

6. Click New Entry Scene in the document outline and click the Connections inspector button. Connect the ratingView outlet to the rating view in the New Entry scene in the storyboard:

![](img/0972ba626734a3986e095c788777818e_437_0.png)

Figure 19.6: Connections inspector showing ratingView outlet

7. Build and run your app. Tap the + button to go to the Add New Journal Entry screen, and you will see the rating view displayed above the Get Location switch. Add a journal entry title, body, and rating, and tap Save:

![](img/0972ba626734a3986e095c788777818e_437_1.png)

Figure 19.7: Simulator showing the rating view on the Add New Journal Entry screen

A new journal entry with a rating is now visible on the Journal List screen. In the next section, you’ll modify the Journal Entry Detail screen to display the rating for this journal entry.

## Add your custom view to the Journal Entry Detail screen

At this point, you’re able to set a rating when you create a new journal entry using the Add New Journal Entry screen, but the rating you set is not visible on the Journal Entry Detail screen. You’ll add an outlet for a rating view and modify the code in the JournalEntryDetailViewController class, and you’ll add a rating view to the Entry Detail scene in the storyboard. Follow these steps:

- 1. In the Project navigator, click the JournalEntryDetailViewController file. Add an outlet for a rating view after all other property declarations:

```
@IBOutlet var photoImageView: UIImageView!
@IBOutlet var mapImageView: UIImageView!
@IBOutlet var ratingView: RatingView!
```

- 2. Modify the code in the viewDidLoad() method to set the value of the rating view’s rating property:

```
super.viewDidLoad()
dateLabel.text = selectedJournalEntry?.date.formatted(
    .dateTime.day().month(.wide).year()
)
ratingView.rating = selectedJournalEntry?.rating ?? 0
titleLabel.text = selectedJournalEntry?.entryTitle
```

- 3. Click the Main storyboard file and click Entry Detail Scene in the document outline. Select the stack view in the second table view cell. Click the Identity inspector button and, under Custom Class, set Class to RatingView:

![](img/0972ba626734a3986e095c788777818e_438_0.png)

Figure 19.8: Identity inspector with Class set to RatingView
