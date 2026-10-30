Launch Excel
Launch Excel and open document SaveAttachmentsFolder \Activity Tracker.xlsx using an existing Excel process and store it into Excel instance ExcelInstance

Get files in folder
Retrieve the files in folder SaveAttachmentsFolder and its subfolders that match *.docx and store them into Files

Get current date and time
Retrieve the current datetime and store it into CurrentDateTime

Figure 1.7 – PAD Excel and filesystem actions

Let’s look at these actions:

+   - **Launch Excel**: Opens Excel and also a specific workbook to store the responses.
+   - **Get Files in a folder**: This action checks the local filesystem and creates a list of files in a specific folder. A filter for files can also be applied. In our case, we only want Word documents with the .docx extension.
+   - **Get Current date and time**: Here, the flow determines the current date and time, which we can subsequently include in the corresponding entry in Excel.

# 3. Handle each document in the attachments folder.

The following block contains the handling for each Word document. The last sentence contains the for each keyword, which is also used in PAD to iterate through a list of items. In the last building block, this list of items/documents has been created by the Get files in folder action and the following block starts with that for each loop, as shown in the following screenshot: