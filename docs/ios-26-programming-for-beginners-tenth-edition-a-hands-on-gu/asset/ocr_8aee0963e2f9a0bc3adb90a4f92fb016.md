Now that you are familiar with the Assist screen, you’ll add the Foundation Models framework and implement the functionality of this screen in the next section.

# Implement the Foundation Models framework

As you have seen, the app displays the prompt you entered in the text field when the Generate button is tapped. To implement the required functionality, first, you’ll need to check whether the system language model is available. Then, you will implement a method to use an instance of LanguageModelSession() to generate text based on your prompt. Finally, you will add code to the button action to call this method and display the resulting generated text in the text view.

> To view Apple’s documentation on generating content and performing tasks with Foundation Models, see this link: https://developer.apple.com/documentation/foundationmodels/generating-content-and-performing-tasks-with-foundation-models.

Using the Foundation Models framework, you will add code so that the app can generate text based on the prompt you entered. Follow these steps:

+   1. Click the AssistViewController file inside the Assist Screen folder in the Project navigator.

+   2. Add code to import the Foundation Models framework after the import statement:

```
import UIKit
import FoundationModels
```

+   3. Add a property that will be assigned an instance of LanguageModelSession() after the outlet declarations:

```
// MARK: - Properties
@IBOutlet var promptTextField: UITextField!
@IBOutlet var resultTextView: UITextView!

private let session = LanguageModelSession(instructions: "Your job is to write a journal entry for the user based on the short description provided.")

// MARK: - View controller lifecycle
override func viewDidLoad() {
```