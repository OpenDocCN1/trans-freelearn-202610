
- X. Drag and drop the Element Exists activity into Enter Registration Details, and name it Check the Continue Button Exists. In the Output field of the properties, create a ContinueAvailable variable. Next, drag and drop the Write Line activity to output the value of the ContinueAvailable variable, as shown in Figure 10.12.

- XI. Check for the availability of the Continue Button and write the output using the Element Exists and Write Line activities, as shown in Figure 10.12:

![](img/84b06ad2832c53c5f677f8ce4c80f184_310_0.png)

Figure 10.12 – Checking for the availability of the Continue button

- XII. Ensure the highlighted properties are selected in all Type Into fields, as shown in Figure 10.13:

![](img/84b06ad2832c53c5f677f8ce4c80f184_310_1.png)

Figure 10.13 – Enabling Type Into options

- 5. Run the process to find the robot, navigate to the **Account details** page, and enter the account details as per your input data, as shown in Figure 10.14:

![](img/84b06ad2832c53c5f677f8ce4c80f184_311_0.png)

Figure 10.14 – Checking the output on the Packtpub page

- 6. Verify the output in the **Output** panel to ensure the **TermsAndConditions** text has been extracted from the web page and the **Continue** button is available as shown in Figure 10.15:

```
Debug started for file: Main
Exploring_UiInteraction execution started
By creating an account, you agree to the Terms And Conditions set out by this site, including our Cookie Use
True
Exploring_UiInteraction execution ended in:
00:00:17
```

Figure 10.15 – Checking the output in UiPath Studio

In this section, we have seen how the robot interacts with a web page to read and write data. We have also seen how the robot interacts with the GUI using automation activities and properties to automate a web application. You have been introduced to the different types of activities and properties you need to be aware of while doing GUI automation. We have also used the activities and properties in an example to understand their functionality better. You will understand more about the different recording options available, and input and output methods in the upcoming sections, which will help you to perform the development faster and perform background automation.

## Methods of recording the GUI

Recording is one of the important features in UiPath Studio, and allows users to capture the actions performed on the screen and convert them into sequences; this saves a lot of time and effort in automating business processes. To start recording, you need to access the **DESIGN (1)** panel and click on **Recording (2)**, which will display all the available recording options (3), as shown in Figure 10.16:

![](img/84b06ad2832c53c5f677f8ce4c80f184_312_0.png)

Figure 10.16 – Different recording options

There are six types of recordings: **Basic**, **Desktop**, **Web**, **Image**, **Native Citrix**, and **Computer Vision**. However, we will be exploring only the first three types (Basic, Desktop, and Web), as those are the most commonly used options and are more relevant for the RPA Associate certification.

### Basic

**Basic** generates a **full selector** for each activity but does not create a container for the activities. This is better suited for creating *linear sequences within a container* and can be used for recording both web applications and desktop applications but is the *slowest* of all recorders.

Let's use the same example of the Packtpub web page to read and write data used in the previous section, but this time using the **Basic** recording:

- 1. Create a new sequence and name it `Recordings`.
- 2. Click on **Recording (1)** and then on **Basic (2)**, as shown in Figure 10.17:

![](img/84b06ad2832c53c5f677f8ce4c80f184_313_0.png)

Figure 10.17 – Launching Basic recording

- 3. The **Basic Recording** panel is displayed with different options and you are able to perform the following actions, as shown in Figure 10.18.

- 4. Record a series of actions or events (such as launch an application, click on a menu, and type into a field), using the **Record** option (1). For this, you need to click on the **Record** button and perform all the required actions, and stop the recording by pressing the *Esc* key once your actions are completed. Perform step-by-step recording (such as launching the application using **Start App**, clicking on a menu using **Click**, and typing into the text field using **Type**) using the different recording tools (2) or you can perform combinations of both options 1 and 2. Press the *Esc* key to stop recording once you have completed all your actions.

![](img/84b06ad2832c53c5f677f8ce4c80f184_313_1.png)

Figure 10.18 – Starting a basic recording

- 5. Click on the **Record** button from the previous screen to record the elements from the **Account details** section of the **Subscription** page at `https://www.packtpub.com/`, in the order of elements numbered as shown in **Figure 10.19**:

![](img/84b06ad2832c53c5f677f8ce4c80f184_314_0.png)

Figure 10.19 – Packtpub Account details section

- 6. Press the *Esc* button on the keyboard or right-click on the mouse to stop recording, and once the recording has been completed, you will find the following, where you can see the option to save your recording or to start a new recording using the various recording options:

![](img/84b06ad2832c53c5f677f8ce4c80f184_314_1.png)

Figure 10.20 – Basic Recording tool

- 7. Click on the **Save & Exit** button for the recorded activities to be saved to the workflow and to exit from **Basic Recording**:

![](img/84b06ad2832c53c5f677f8ce4c80f184_315_0.png)

Figure 10.21 – Basic Recording Save & Exit

- 8. Verify whether the username and password were generated from the recording, as shown in Figure 10.22:

![](img/84b06ad2832c53c5f677f8ce4c80f184_315_1.png)

Figure 10.22 – Workflow verification of username and password

- 9. Verify whether **First name**, **Last name**, and the **Continue** button were recorded, as shown in Figure 10.23:

![](img/84b06ad2832c53c5f677f8ce4c80f184_316_0.png)

Figure 10.23 – Workflow verification, other recorded fields

- 10. Click on the **Options** menu (1) and then click on **Edit Selector** (2) from one of the activities, as shown in Figure 10.24:

![](img/84b06ad2832c53c5f677f8ce4c80f184_317_0.png)

Figure 10.24 – Edit Selector menu option

- 11. Verify the Edit Selector section to ensure that the full HTML fragment of the selector is displayed and editable. Also, verify the browser details displayed; this is the reason we call the Basic recording a full selector as shown in Figure 10.25. Note that the HTML fragment section is non-editable in partial selectors.

![](img/84b06ad2832c53c5f677f8ce4c80f184_317_1.png)

Figure 10.25 – Edit Selector – Full selector

- 12. Note that even though it is recorded from a web application, there are no containers, such as Open Browser or Attach Browser, recorded within the Basic sequence, as shown in Figure 10.22.

Now, let's explore another example of basic recording, where the robot enters the text UiPath into Notepad. To perform that, follow these steps to generate the **Basic** sequence, as shown in Figure 10.26:

- 1. Launch Notepad.
- 2. Click on the **Basic Recording** option from UiPath Studio.
- 3. Click on the **Type** option from the Basic Recording tool (2) as shown in Figure 10.18 during the recording and navigate the cursor to the Notepad.
- 4. Type UiPath in Notepad.
- 5. Press the *Esc* button to stop recording.
- 6. Click on the **Save & Exit** button.

![](img/84b06ad2832c53c5f677f8ce4c80f184_318_0.png)

Figure 10.26 – Type into Notepad application (Basic recording)

- 7. Click on the **Options** menu (1) and then click on **Edit Selector** (2) from one of the activities, as shown in Figure 10.27:

![](img/84b06ad2832c53c5f677f8ce4c80f184_319_0.png)

Figure 10.27 – Edit Selector

- 8. Verify the **Edit Selector** box of the Type Into activity to ensure the HTML code generated follows the structure of a **full selector**, which means each and every attribute within the HTML tags is editable as shown in *Figure 10.28*:

![](img/84b06ad2832c53c5f677f8ce4c80f184_319_1.png)

Figure 10.28 – Selector Editor view of a full selector (Type Into Notepad)
