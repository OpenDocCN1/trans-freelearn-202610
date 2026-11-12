# # Activities

UI Automation activities are used to develop **tasks** or **workflows** for the robot to communicate with the GUI of several applications. Activities are essentially classified into four different types. These are shown in *Figure 10.5*:

| Activities | Description |
| :--- | :--- |
| **Containers** | These are used to locate any GUI applications (desktop, web, or mainframes) or to identify active windows or pop up windows to communicate with the elements within the scope of the selected window. Open Browser, Open Application, Attach Browser, and Attach Window are examples of containers. |
| **Input** | These activities are used to provide inputs or perform actions on the UI elements. Examples are Click, Type Into, and Send Hotkeys. |
| **Output** | These activities instruct the robot to extract data from the GUI. Examples are GetText, GetURL, and GetOCRText. |
| **Synchronization** | These activities instruct the robot to perform defined operations based on the GUI behaviors to overcome any unexpected UI behavior. Examples are Element Exists, Image Exists, and Wait Element Vanish. |

Figure 10.5 – Types of UI Automation activities

# # Properties

The properties of the activity decide how the robot will interact with the GUI based on the activity used. The different properties options can be found in the **Properties** panel section, usually displayed by default on the right-hand side of UiPath Studio. Let's explore some key properties used in GUI automation as follows:

+   * **DelayAfter** and **DelayBefore**: Instructs the robot to wait for a specified time in milliseconds before or after performing the activity. The default amount of delay time for **DelayAfter** is 300 milliseconds, and the wait time for **DelayBefore** is 200 milliseconds. You may increase or decrease the delay time depending on the responses of the UI element.
+   * **ContinueOnError**: Instructs the robot to wait for a specified time in milliseconds before or after performing the activity. By default, **ContinueOnError** is **False**, which means the remaining activities will not be executed in case of any errors or exceptions. You may select this as **True** if you want to continue with the execution despite an application exception.