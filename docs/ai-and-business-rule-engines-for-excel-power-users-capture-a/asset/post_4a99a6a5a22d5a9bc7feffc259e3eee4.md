
+   - Office Scripts is highly integrated with Power Automate – a tool that we’ll meet in the next chapter and will allow you to automate a lot of manual steps not just in Excel.

While there are benefits to using Office Scripts, it also has some disadvantages:

- Microsoft is still rolling out Office Scripts, and it is unclear from the roadmap when it will be available to personal (non-commercial) users.
- Office Scripts can only be edited in the web version of Excel. In practice, the editor is good enough (and backed up by the more powerful VS Code online editor) that this is not a major issue. Completed scripts *can* be run on the web and desktop versions of Excel.
- Office Scripts does not have the full set of user interface options that Script Lab does.

In the end, the choice of how to link Excel to your decision service will depend on what else you want to do with the tool. Let’s help with making that choice by taking a tour of Office Scripts.

## Getting familiar with the Office Scripts environment

To check whether you have Office Scripts available to you, open the online (Excel 365) version. If it has been enabled by an administrator, you should see an **Automate toolbar** option, as in *Figure 7.13*.

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_201_0.png)

Figure 7.13 – Office Scripts toolbar

Much of the layout of Office Scripts is similar to Script Lab. The toolbar has **New Script** and **Sample Script Areas**. The **Automate a Task** button links to Power Automate (more about this in *Chapter 8*).

The **Record Actions** button works similarly to the tool of the same name in VBA. Clicking on it captures the actions you take in Excel, as a script, in a pane on the right-hand side of the screen. The following script example is of a recording we made while setting some values in our Excel sheet:

```
```typescript
function main(workbook: ExcelScript.Workbook) {
  let selectedSheet = workbook.getActiveWorksheet();
  // Set range B2 on selectedSheet
```
```

```
selectedSheet.getRange("B2").setValue("This is a test");
// Set fill color to FFFF00 for range B2 on selectedSheet
selectedSheet.getRange("B2").getFormat().getFill().
setColor("FFFF00");
}
```

The pane on the right-hand side of the screen is laid out similarly to Script Lab – with a code editor, an area with the log messages from running code, and several other useful command buttons.

The code editor has a Save button. Files are saved in your OneDrive folder under Documents | Office Scripts. It's not an automatic save (unlike Script Lab), so make sure you save before running.

The ... menu in the right-hand panel gives the Add button option. Selecting this (unsurprisingly) adds a button to the current workbook that will run the script, as in Figure 7.14.

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_202_0.png)

Figure 7.14 – Sharing our script via a button in our workbook

More importantly, creating this button automatically shares our script with people who can view the workbook. This is probably the easiest way to make sure our workbook and script get distributed together. But there are more detailed sharing options available in the Share menu option of the ... menu.

Since much of the layout of Office Scripts is similar to Script Lab, we've kept the descriptions brief. A good starting point to learn more is the Office Scripts documentation at https://learn.microsoft.com/en-us/office/dev/scripts/.

Let's prove that the two scripting approaches are similar by modifying our previous Script Lab example to work in Office Scripts.

## Calling our decision service from Office Scripts

When I wrote this section, converting the previous example to work in Office Scripts took less than 10 minutes. Rather than repeating the previous steps, we’ll show how to import the updated example into Office Scripts, then run through and highlight the changes that we’ve had to make:

1. Download the full script from the usual GitHub site. The example filename is 07_Office_Script_Decision_Service.txt. Note the .txt extension, as we have given the same example in two different formats.
2. Open the file in an editor such as Notepad and copy all the contents.
3. In the online version of Excel, select the Automate menu, then New Script, and paste the contents of the file to replace all the previous contents of this window.
4. Save the result.

If you’re more familiar with where Office Scripts saves files, it’s possible to download the example directly into the OneDrive folder under Documents | Office Scripts. Note the format of the file is slightly different – it’s the 07_Office_Script_Decision_Service.osts file that you need. Note the .osts extension.

If downloading the example doesn’t work, check that you have the right file for the method you are trying to use.

The biggest change in the example is since Office Scripts has no user interface elements, we’ve removed those from the sample. While the values we pass into the decision service are hardcoded to keep things simple, it would be easy to read these from a cell value instead.

Other key changes in the script worth highlighting are as follows:

- Office Scripts hides a lot of complexity from us. We have a simple main method, which always takes ExcelScript.Workbook as a parameter so the script can manipulate the current workbook.
- Since the sample uses TypeScript, we need to label the types of objects the script is using – hence, the workbook tag in our main method:
- Our script set the URL, the headers, the JSON payload, and the URL and carries out the actual REST call using Fetch the same as before. We also select the product recommendation from the values returned in the same way.
- The script to (re)create the worksheet and headers and display the product recommendation is broadly the same. Any small differences are due to using a slightly different version of the Office.js API.
- Error handling is built in automatically, so we don’t need to write our own function to do this.

```
async function main(workbook: ExcelScript.Workbook) {
```

The script is well commented and it’s worth reading through it before pressing the Run button to get it started. After you click on the button, you should get a message saying Run Succeeded, and you can view the updated Excel sheet, as in Figure 7.15:

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_204_0.png)

Figure 7.15 – Excel sheet online, updated with decision service results

Since we kept the logging from our previous example, the script output information window gives a lot of information about what just happened, as in Figure 7.16. Again, each of these steps is the same as our Script Lab example.

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_204_1.png)

Figure 7.16 – Script logging output

We’ll come back to Office Scripts in Chapter 8. But for the moment, congratulations! If you ran this example, along with the previous ones, you now have four different ways of linking Excel to your Kogito decision service (and know of two other methods). This now gives you a difficult choice – which method should you use?

## Lots of choices – but which one to link with?

There is a good reason why we have mentioned a lot of different approaches to linking Excel to your decision service. There is no one best method – your choice will depend on what else you are trying to achieve with your project. This table might help you decide:

| Tool | Advantage | Disadvantage | Use when |
|---|---|---|---|
| Power Query | Built into Excel. Acts as a powerful function. | Users can break it. No UI elements. | Default method to get started. |
| Visual Basic | Very powerful. Includes UI elements. | Security warnings scare users. Outdated. | Using a legacy code base or team has VBA. |
| Script Lab | Modern version of Visual Basic based on web technologies. | Needs installing. | Looking for a rich user experience or if Office Scripts is not available. |
| Functions in Script Lab | Easiest to use – feels like a normal Excel function. | Needs three items aligned to work (Script Lab, the function to be registered, and the function to be in the workbook). | Niche – consider Power Query instead. |
| Office Scripts | Simpler than Script Lab. No installation needed. | Not available to all users. No UI elements. | After you outgrow Power Query – if it is available to you. |
| Power Automate | External to Excel. Harder for users to break. | More powerful but more complicated. | Need to call the decision service as part of wider workflow. |

Table 7.1 – Different methods for linking Excel and your decision service

If you’re finding it difficult to choose, here’s some good news – any step you take to move your business rules out of Excel and into a decision service such as Kogito is a good one.
