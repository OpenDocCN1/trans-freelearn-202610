You’ll implement these methods in your AddJournalEntryViewController class now. In the Project navigator, click on the AddJournalEntryViewController file. Type the following code in the UIImagePickerControllerDelegate extension:

```
// MARK: - UIImagePickerControllerDelegate
func imagePickerControllerDidCancel(_ picker: UIImagePickerController) {
    dismiss(animated: true)
}
func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]) {
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