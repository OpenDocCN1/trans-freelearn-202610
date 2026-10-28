```
let textViewText = bodyTextView.text ?? ""
saveButton.isEnabled = !textFieldText.isEmpty &&
!textViewText.isEmpty
}
```

The private keyword means that this method is only accessible within this class.

+   5. Add the UITextFieldDelegate method shown after the textFieldShouldReturn(_:) method:

```
func textFieldShouldReturn(_ textField: UITextField) -> Bool {
    textField.resignFirstResponder()
    return true
}
func textFieldDidEndEditing(_ textField: UITextField) {
    updateSaveButtonState()
}
```

This method calls updateSaveButtonState() after the text field resigns the first responder status.

+   6. Add the UITextViewDelegate methods shown after the textView(_:shouldChangeTextInRanges:replacementText:) method:

```
func textView(_ textView: UITextView, shouldChangeTextInRanges ranges: [NSValue], replacementText text: String) -> Bool {
    if (text == "\n") {
        textView.resignFirstResponder()
    }
    return true
}
func textViewDidEndEditing(_ textView: UITextView) {
    updateSaveButtonState()
}
func textViewDidChange(_ textView: UITextView) {
    updateSaveButtonState()
}
```