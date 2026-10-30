Here, we make use of the different variables that were created previously. They make the settings much more readable than using table variables with indexes.

At this point, the loop ends and starts over again for each of the records in that Excel workbook. After that, Outlook and Excel are closed via their corresponding actions. SAPGUI can be closed by a keyboard command that is common to all Windows programs: *Alt + F4*. After that, the program will want a confirmation to log off, which is also provided with another keystroke.

# Additional thoughts and automation options

Of course, for every challenge, there are usually several possible solutions and the approach presented here is not the only one. It would also be possible, for example, to open Outlook only after processing the data in SAP and sending the emails to the users. This would probably also be the better variant for computers with little RAM. And as also mentioned, SAPGUI can be automated in other ways as well.

Microsoft has recently added extensive information about SAPGUI integration and automation to its documentation, which also underlines the importance of this possibility once again.

Here, we will talk about multiple implementation options:

+   - **Pro-code**: In this approach, VBScript is used to interact with the underlying SAPGUI Scripting API. Here, actions to be executed are recorded by SAPGUI in the form of VBScript commands, which can then be imported and used in PAD. This sounds simple at first. However, unless you happen to be an expert in this scripting language, changes in the process will be difficult to implement.
+   - **Low-code/no-code**: These two approaches use the recorder to capture specific fields such as buttons and text fields, which are tweaked