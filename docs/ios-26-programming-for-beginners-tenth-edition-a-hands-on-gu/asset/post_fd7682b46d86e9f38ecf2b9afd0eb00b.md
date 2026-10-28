
4. Click the Attributes inspector button and verify the settings under Stack View. Under View, uncheck the User Interaction Enabled checkbox (as the user should not be able to change the rating on the Journal Entry Detail screen), and set Background to Default:

![](img/0972ba626734a3986e095c788777818e_439_0.png)

Figure 19.9: Attributes inspector with Background set to Default

5. Click **Entry Detail Scene** in the document outline and click the Connections inspector button. Connect the **ratingView** outlet to the rating view in the **Entry Detail** scene in the storyboard:

![](img/0972ba626734a3986e095c788777818e_440_0.png)

Figure 19.10: Connections inspector showing ratingView outlet

6. Build and run your app. Tap the journal entry you added in the previous section, and you’ll see the rating displayed in a rating view on the Journal Entry Detail screen:

![](img/0972ba626734a3986e095c788777818e_440_1.png)

Figure 19.11: Simulator showing the rating view on the Journal Entry Detail screen

You’ve successfully added and configured a rating view on the Journal Entry Detail screen! Good job!

## Technical requirements

You will continue working on the JRNL project that you modified in the previous chapter.

The resource files and completed Xcode project for this chapter can be found in the Chapter20 folder of the code bundle for this book, which can be downloaded here:

https://github.com/PacktPublishing/iOS-26-Programming-for-Beginners-10E

Check out the following video to see the code in action:

https://youtu.be/UY9AB8TLjOU

Let’s start by modifying the Add New Journal Entry screen to display an image picker controller, which allows you to use the device camera or select a photo from the user’s photo library.

## Create a new UIImagePickerController instance

To make it easy for a user to use the camera or photo library, Apple implemented the UIImagePickerController class. This class manages the system interfaces for taking photos and choosing items from the user’s photo library. An instance of this class is called an image picker controller, and it can display an image picker on the screen.

If you have ever added a photo to a social media post, you will have seen what the image picker looks like. It typically displays either the view from your camera or a grid of photos from your photo library, and you can then choose a photo to be added to your post:

![](img/0972ba626734a3986e095c788777818e_444_0.png)

Figure 20.1: Simulator showing the image picker

5. You should see the contents of the AddJournalEntryViewController file in the assistant editor. Ctrl + Drag from Tap Gesture Recognizer in the document outline to the space between the locationSwitchValueChanged(_:) method and the closing curly brace:

![](img/0972ba626734a3986e095c788777818e_447_0.png)

Figure 20.5: Editor area showing drag destination

6. In the pop-up dialog box, set Name to getPhoto and Type to UITapGestureRecognizer. Click Connect:

![](img/0972ba626734a3986e095c788777818e_447_1.png)

Figure 20.6: Pop-up dialog box for action creation

2. Modify the getPhoto() method as shown:

```
@IBAction func getPhoto(_ sender: UITapGestureRecognizer) {
    let imagePickerController = UIImagePickerController()
    imagePickerController.delegate = self
    #if targetEnvironment(simulator)
    imagePickerController.sourceType = .photoLibrary
    #else
    imagePickerController.sourceType = .camera
    imagePickerController.showsCameraControls = true
    #endif
    present(imagePickerController, animated: true)
}
```

Let’s break this down:

```
let imagePickerController = UIImagePickerController()
```

This statement creates an instance of the UIImagePickerController class and assigns it to imagePickerController.

```
imagePickerController.delegate = self
```

This statement sets the image picker controller’s delegate property to the AddJournalEntryViewController instance.

```
#if targetEnvironment(simulator)
imagePickerController.sourceType = .photoLibrary
#else
imagePickerController.sourceType = .camera
imagePickerController.showsCameraControls = true
#endif
```

This block of code is known as a conditional compilation block. It starts with an #if compilation directive and ends with an #endif compilation directive. If you’re running in Simulator, only the statement setting the image picker controller’s sourceType property to the photo library is compiled. If you’re running on an actual device, the statements setting the image picker controller’s sourceType property to the camera and displaying the camera controls are compiled. This means that the image picker controller will use the photo library when running in Simulator and will use the camera when running on an actual device.

You can learn more about conditional compilation blocks at this link: https://docs.swift.org/swift-book/documentation/the-swift-programming-language/statements/#Conditional-Compilation-Block.

```
present(imagePickerController, animated: true)
```

This statement presents the image picker controller on the screen.

You’ve implemented all the code required to present the image picker controller when the image view is tapped. In the next section, you’ll implement the UIImagePickerControllerDelegate methods that will be called when the user chooses an image or cancels.

## Implement UIImagePickerControllerDelegate methods

The UIImagePickerControllerDelegate protocol has a set of methods that you must implement in your delegate object to interact with the image picker controller interface.

To learn more about the UIImagePickerControllerDelegate protocol, see https://developer.apple.com/documentation/uikit/uiimagepickercontrollerdelegate.

When the image picker controller appears on screen, the user has the option of selecting a photo or canceling. If the user cancels, the imagePickerControllerDidCancel(_:) method is triggered, and if the user selects a photo, the imagePickerController(_:didFinishPickingMediaWithInfo:) method is triggered.

You’ll implement these methods in your AddJournalEntryViewController class now. In the Project navigator, click on the AddJournalEntryViewController file. Type the following code in the UIImagePickerControllerDelegate extension:

```
// MARK: - UIImagePickerControllerDelegate
func imagePickerControllerDidCancel(_ picker: UIImagePickerController) {
    dismiss(animated: true)
}
func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey: Any]) {
    guard let selectedImage = info[.originalImage] as? UIImage else {
        fatalError("Expected a dictionary containing an image, but was provided the following: \(info)")
    }
    let smallerImage = selectedImage.preparingThumbnail(of: CGSize(width: 300, height: 300))
    photoImageView.image = smallerImage
    dismiss(animated: true)
}
```

The imagePickerControllerDidCancel(_:) method is triggered when the user cancels. The image picker controller is dismissed and the user is returned to the Add New Journal Entry screen.

The imagePickerController(_:didFinishPickingMediaWithInfo:) method is triggered when the user selects a photo. This photo is then assigned to selectedImage. Next, the selectedImage instance’s preparingThumbnail(of:) method will be used to create a smaller image with a width and height of 300 points, the same as the size of the image view on the Journal Entry Detail screen. This image will then be assigned to the photoImageView property and the image picker controller will be dismissed.

You can learn more about the preparingThumbnail(of:) method at this link: https://developer.apple.com/documentation/uikit/uiimage/3750835-preparingthumbnail.

All the required `UIImagePickerController` delegate methods have been implemented. In the next section, you’ll modify the `Info.plist` file so that your app will ask for permission to use the camera or photo library.

## Get permission to use the camera or photo library

Apple stipulates that your app must inform the user if it wishes to access the camera or photo library. If you don’t do this, your app will be rejected and will not be allowed on the App Store.

You’ll modify the Info property list file in your project to make your app display messages when it tries to access the camera or photo library. Follow these steps:

1. Click on the **Info** property list file in the Project navigator. Move your pointer over the **Information Property List** row and click on the + button to create a new row.

2. In the new row, set **Key** to **Privacy - Photo Library Usage Description** and **Value** to `This app uses photos in your photo library when creating journal entries.`

3. Add a second row using the + button. This time, set **Key** to **Privacy - Camera Usage Description** and **Value** to `This app uses your camera when creating journal entries.` Your Info.plist file should look like the following when done:

4. Build and run your app. Go to the Add New Journal Entry screen and tap the image view. The image picker will appear:

![](img/0972ba626734a3986e095c788777818e_452_0.png)

Figure 20.8: Info.plist with additional keys added
