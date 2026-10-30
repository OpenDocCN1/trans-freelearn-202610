
## Distributed user management with PAD

Let’s imagine that we are responsible for setting up users who will start in our small company next month. Besides a login name and a mailbox, of course, an SAP login is also needed. The HR department has already created accounts in Active Directory and collected the users for them in an Excel spreadsheet. Our task is to create the users listed there in SAP and to send the login information by email. This also includes creating an initial password. So, we need a flow that does the following for us:

- Read all users from the Excel spreadsheet and extract data
- Launch SAPGUI and call the corresponding transaction to create new users (SU01)
- Iterate through the Excel data, generate a new password, store this in the corresponding row of the table, and use the data to create a user in SAP
- Send an email to the user with the initial password

### Store the Excel worksheet and close Excel and SAPGUI

Since we don’t have separate actions for automating SAPGUI, we have to use the techniques we already know to control this application remotely. However, since this application can also be operated excellently via keyboard input, we want to avoid creating UI elements and images as much as possible. Using keyboard input as the option to navigate through the application can sometimes be an advantage because this way, the user would not rely on screen resolutions and the need to capture all UI elements upfront.

Also, this time, we want to apply our proven practice of dividing the overall flow into smaller sub-flows. The complete flow will look like this:

As the name suggests, this area starts SAPGUI, which must be installed on your computer. The program is called directly in the first action. The program consists of two windows – one for selecting the system to log in to and another for logging on and running programs. To be able to address these windows in the further process directly, these are defined in the form of UI elements. To do this, it is a good idea to start the program once manually and define the two windows as UI elements via PAD.

To start the SAPGUI program, we could implement a flow like this:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_226_0.png)

Figure 8.3 – Starting SAPGUI with PAD

First, we must run the main `saplogon.exe` executable through the **Run application** action (line 1). This executable is available after the standard installation of the SAP client software. This is essentially the same as double-clicking on the corresponding shortcut on the Windows desktop. In line 2, we wait for the window to appear; after that, we will see the **SAP Logon** screen, where we can select a system to log on with:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_227_0.png)

Figure 8.4 — SAP system selection

As we can see, I only have one system registered. That’s why I just need to press *Enter* here by sending a keystroke (line 3). If you have multiple systems registered and want to log on to a specific system, you will need to make sure that the right entry is selected first.

In line 4, we wait for the main SAP window to appear, which looks like this:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_228_0.png)

Figure 8.5 – SAP logon screen

As mentioned earlier, this software has also been designed to be used just using keystrokes. We can see where the cursor is located by the light red corners of the UI elements – in this case, the text box for the username. Again, this is the default behavior of that software and we could just make use of this. However, if we need to log on to a different client (see the Client field in the preceding screenshot), we would need to make sure that these settings are made.

In lines 5 and 6 in our flow, we enter the username and password. To jump from one field to the next, we can just use the Tab key. These values are stored in two input variables and are marked as sensitive. The last step is to press Enter and wait for the login procedure to finish (lines 6 and 7). At this stage, we have successfully logged on to the system and can start working with programs in SAP.

### The “Launch_Excel” and “Launch_Outlook” sub-flows

The next steps in our main flow are to look up the users that we need to register in our SAP system and then send them their user information via

Because we want to store a generated password in the corresponding row for a user, we do not close Excel at this point. From here, we can start processing each user.

### User data processing

Now that we’ve launched SAPGUI alongside Outlook and Excel, we are ready to process the data for each of the users. The first thing we have to do is call a specific transaction in SAP called SU01 – this is exactly what we do in line 4 of our main flow.

### SAP and transaction codes

When working with SAP ERP, we often talk about so-called reports and transactions, which are used to execute business requirements and processes. A transaction code (for example, SU01) starts a program for entering parameters and very often starts a report afterward. The transaction used here, for example, allows a username to be entered, which can either be searched for or created. Therefore, the process shown here represents a very typical routine for working with SAP.

The following screenshot shows the action parameter dialog for this step:

**Send keys**

Sends keys to the application that is currently active (More info)

**Select parameters**

| Parameter | Value |
| --- | --- |
| Send keys to | UI element |
| UI element | `appmask > Window 'SAP'` |
| Text to send | Input as text, variable or expression {x} |
|  | `su01{Return}` |
| Insert special keys | (button) |
| Insert modifier | (button) |
| Delay between keystrokes | 10 |
| Send Text as hardware keys | |
| On error | |
| Save | (button) |
| Cancel | (button) |

Figure 8.7 – Calling SAP transaction “SU01”

The reason we are looking at this step a little more closely here is that we will use this basic concept throughout this example:

- Because we can also select a UI window that we have previously defined with this action, we do not have to pay attention to whether this window is also in the foreground. This is especially useful when working with several programs in a flow and it is not always possible to know which window is currently in the foreground. By specifying the UI element, the keystrokes are sent to the specified window – in our case, SAPGUI.
- We can accomplish our task entirely through keyboard input, making us independent of additional UI elements such as buttons or list items. Thus, this flow also works independently of screen resolutions.
- The PAD action also allows us to perform special keystrokes such as *Shift + Tab* (jumps one field back in the interface) or *Ctrl + S* (shortcut to save, also in SAP).
- In addition to this, we can specify the time between keystrokes in milliseconds. This is useful when running SAPGUI on slower computers.

By executing this action, we will land on the screen for maintaining users in SAP. Next, we go through the loop (lines 5 to 9) in which we complete the following subtasks:

- Determine the user information to be processed (the `Read_ExcelUserRow` sub-flow). We read the table of users before.
- Using the information to enter it in SAPGUI (the `Enter_SAPUserData` sub-flow).
- Send an email to the user with initial login credentials.

For the loop itself, we are not iterating through the list of rows with the `For each` loop. Instead, we can use the index-based method because this makes it easier to write back data into the right row in Excel. Let’s take a look at the details of the `Read_ExcelUserRow` sub-flow and see how this works:

Figure 8.8 – Reading user data and generating a random password

Please note that we create a random password for the logon immediately in line 1. We use the Create random text action from the Text action group. This action was made for this purpose because possibilities such as length and complexity can be defined. In lines 2 to 6, we create flow variables to make it easier for us to work with this data later in the flow. Line 7 is responsible for saving the generated password in the corresponding line in the Excel worksheet. Here, we make use of the LoopIndex variable, which contains the number of the row we are processing right now.

Subflows: Read_ExcelUserRow, Enter_SAPUserData, Launch_Outlook

| Step | Action | Description |
| --- | --- | --- |
| 1 | Create random text | Create a random text of length between 8 and 10 characters using uppercase letters, lowercase letters, symbols and digits and store it in InitialPassword |
| 2 | Set variable | Assign to variable UserName the value ExcelData[LoopIndex][0] |
| 3 | Set variable | Assign to variable UserFirstName the value ExcelData[LoopIndex][1] |
| 4 | Set variable | Assign to variable UserLastName the value ExcelData[LoopIndex][2] |
| 5 | Set variable | Assign to variable UserEmail the value ExcelData[LoopIndex][3] |
| 6 | Set variable | Assign to variable UserAlias the value ExcelData[LoopIndex][4] |
| 7 | Write to Excel worksheet | Write the value InitialPassword into cell in column 6 and row LoopIndex + 2 of the Excel instance ExcelInstance |

In the next sub-flow, we are using SAPGUI again and operating the program to create a new user. As already mentioned, this is entirely possible
