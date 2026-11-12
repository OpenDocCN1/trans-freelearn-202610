
18. Which of these panels displays the information about an activity yet to be executed, along with its parent containers when a project is paused during debugging?

- A. **Locals panel**
- B. **Call Stack panel**
- C. **Watch panel**
- D. **Immediate panel**

19. What happens if you rename a variable from the **Variables** panel?

- A. You will have to change the name at all places where the variable is used.
- B. The name will be automatically updated in all the activities that use it.
- C. Errors and exceptions will be thrown during execution.
- D. The variable name cannot be edited from the **Variables** panel.

20. How can you improve the following calendar page selector to work only for the month of May in 2022?

The selector code is as follows:

```
"<html app='chrome.exe' title='UiPath - Calendar - Week of May 1, 2022' />"
```

- A. "<html app='chrome.exe' title='UiPath - Calendar - * 2022' />"
- B. "<html app='chrome.exe' title='UiPath - Calendar - * />"
- C. "<html app='chrome.exe' title='UiPath - Calendar - Week of ?????, 2022' />"
- D. "<html app='chrome.exe' title='UiPath - Calendar - Week of May *, 2022' />"

21. Which activity should Brock use to write a data table into a string variable?

- A. For Each Row
- B. Merge Data Table
- C. Generate Data Table
- D. Output Data Table

22. What is the variable type in the Output property field of all Get IMAP Mail Messages activities?

- A. MailMessage
- B. Array<MailMessage>
- C. List<MailMessage>
- D. MailMessages

## 18 Mock Exam 2

This is mock exam 2. It is used to test your knowledge and give you a feel for what the actual exam is like. This mock exam does not contain the actual exam questions. If you feel you are ready, take this exam without looking at the solutions. If you get a score of 70% or above on this one, you can be sure you are ready to take the actual exam, and you will have a really good chance of passing.

So, what are you waiting for? Go for it! Answer the following questions:

- 1. Which of the following processes should not be chosen for RPA?
- A. Rule-based
- B. Voluminous
- C. Repetitive in nature
- D. A process with a high exception rate

- 2. How can execution be paused before a particular activity while debugging?
- A. By using a Break activity
- B. By using a MessageBox activity
- C. By using a breakpoint in Debug mode
- D. By using a Wait Attribute activity

- 3. Which key combination allows you to automatically create an Out direction argument from an activity's property field?
- A. Ctrl + Shift + E
- B. Ctrl + Shift + M
- C. Ctrl + K
- D. Ctrl + P

- 4. Can you store a selector in a variable?
- A. No.
- B. Yes, but the data type should be Boolean.
- C. Yes, but the datatype should be String.
- D. Yes, but the datatype should be Int32.

- 5. An array of String named countries has been initialized with the following:
{"India","Dubai","Singapore","Nepal","Sweden","Maldives"}.
The Variable panel is as follows:

| Name | Variable type | Scope | Default |
|---|---|---|---|
| countries | String[] | Sequence | {"India","Dubai","Singapore","Nepal","Sweden","Maldives"} |

## The Main.xaml workflow is as follows:

![](img/84b06ad2832c53c5f677f8ce4c80f184_529_0.png)

Based on the graphics, which country names are printed by the **Write Line** activity in the **Output** panel?

- A. India
  Dubai
  Singapore
  Nepal
- B. India
- C. India
  Dubai
  Singapore
- D. India
  Dubai
  Nepal

- 6. Which of the following `string` methods will return the zero-based index of the first occurrence of a character in a string?
- A. Concat
- B. Format
- C. IndexOf
- D. SubString

- 7. A file called `Inspection.xlsx` has many duplicate entries. If the developer wants to remove the duplicates directly from the `Inspection.xlsx` file, which activity should be used?
- A. Remove Data Row
- B. Remove Duplicate Rows
- C. Remove Data Column
- D. Remove Duplicates Range

- 8. Queue items have many statuses that they go through. What does the **In Progress** status refer to?
- A. The item has just been added to the queue with the **Add Queue Item** activity.
- B. The item was added due to the failure of a previous queue item with auto-retry enabled.
- C. The item was processed and sent to a **Set Transaction Status** activity.
- D. The item was processed with the **Add Transaction Item** activity.

- 9. Which UiPath components ensure automatic workload distribution across Robots?
- A. Robot
- B. Studio
- C. Orchestrator
- D. State Machine

- 10. What is the **Manage Packages** option of the Design Ribbon bar used for?
- A. It enables you to download activity packages, libraries, and frameworks and update them, as well as add and remove them.
- B. It enables you to download activity packages, libraries, and frameworks, view the ones already installed on your computer, and update them.
- C. It enables you to download activity packages, libraries, and frameworks, view the ones already installed on your computer and update them, as well as add and remove them.
- D. It enables you to download activity packages and frameworks, view the ones already installed on your computer and update them, as well as add and remove them.

- 11. A developer has created an automation process that includes Main.xaml, as shown in the following figure:

![](img/84b06ad2832c53c5f677f8ce4c80f184_531_0.png)

The Main.xaml file invokes the Number_Assign.xaml workflow using the following arguments:

| Name | Direction | Type | Value |
|---|---|---|---|
| out_num1 | Out | Int32 | num1 |
| out_num2 | Out | Int32 | num2 |

The Main.xaml file invokes the Addition.xaml workflow using the following arguments:

| Name | Direction | Type | Value |
|---|---|---|---|
| in_num1 | In | Int32 | num1 |
| in_num2 | In | Int32 | num2 |
| out_sum | Out | Int32 | sum |

The Number_Assign.xaml workflow is as follows:

![](img/84b06ad2832c53c5f677f8ce4c80f184_532_0.png)

The Addition.xaml workflow is as follows:

![](img/84b06ad2832c53c5f677f8ce4c80f184_533_0.png)

Based on the information shown in these figures, what is the output of the MessageBox activity in the Main.xaml file?

- A. 80
- B. 5
- C. 8
- D. 50

- 12. What role does the Highlight button in the UI Explorer window have?
- A. It indicates a new UI element to replace the previous one.
- B. It brings the target element to the foreground.
- C. It enables you to choose an anchor relative to the target UI element.
- D. It shows the status of the selector by checking the validity of the selector definition and the visibility of the target element on the screen.

- 13. When is the Finally block of a Try/Catch activity executed?
- A. When the activities in the Try block are executed with no error
- B. When the activities in the Try block are executed and have errors
- C. Every time, regardless of whether an exception occurred or not
- D. When the activities in the Catch block are executed and have errors

- 14. A developer has created an automation process, as shown in the following figure:

![](img/84b06ad2832c53c5f677f8ce4c80f184_534_0.png)

Based on the information shown in the figure, what is the output of the Message Box activity?
- A. icken
- B. cken Bi
- C. icken B
- D. cken B

- 15. What are the only types of delimiters supported in the Read CSV activity?
- A. Tab, comma (,), semicolon (;), caret (^), and pipe (|)
- B. Tab, comma (,), semicolon (;), slash (/), and braces ({ })
- C. Tab, comma (,), semicolon (;), slash (/), and pipe (|)
- D. Tab, comma (,), semicolon (;), caret (^), and braces ({ })

- 16. Out of the following options, when does a queue item go into the revision types of statuses?
- A. When the item was processed and sent to a Set Transaction Status activity
- B. When the item remained in the In Progress status for an extended period (approximately 24 hours) without being processed
- C. When the item has been manually selected from the Transactions page and marked as deleted
- D. When the transaction item is postponed

- 17. Which UiPath components are used for designing complex attended, unattended, and testing automation?
- A. Robot
- B. Studio
- C. Orchestrator
- D. State Machine

- 18. Test Bench for an activity can be created from where?
- A. Designer panel
- B. Activities panel
- C. Locals panel
- D. Watch panel

- 19. How can you pass data from one workflow to another?
- A. By using variables
- B. By using activities
- C. By using arguments
- D. By using packages
