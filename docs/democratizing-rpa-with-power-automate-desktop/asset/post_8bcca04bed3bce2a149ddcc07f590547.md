
Figure 1.8 – Handling each document in a loop

The actions are as follows:

- **Rename file(s):** Used to rename one or more files. Since we are iterating through the list of Word documents, each iteration contains a different file in the folder. To identify the current file, this gets renamed to `work-in-progress.docx`.
- **Run application:** PAD does not have specific actions for Microsoft Word, but like any other application, this program can also be automated. With this action, we start Microsoft Word on the local machine with a specific document, called `work-in-progress.docx`, which is the current document.
- **Get selected checkboxes in window:** This action can extract `true` (for checked) or `false` (for unchecked) from any window of an application. In our example, we extract the information from the checkboxes of the Word document. We need this for the two checkboxes in the document, which is based on a template that was generated and sent in advance.
- **Extract data from window:** Similar to a checkbox, this action allows us to extract text data from a window. We use this to extract the name of the child in the document.
- **Get first free column/row from Excel workbook:** The workbook has already been opened in one of the actions. This action helps activate the first empty cell so that we can put content in it.
- **Write to Excel worksheet:** This action writes data to the Excel cell specified. We need to perform this action four times to put all the data in.
- **Terminate process:** This closes the Word application so that it can be opened with the next document in the next iteration.

### 4. Housekeeping.

Finally, we need to do some housekeeping, which will be covered by the actions shown in the following screenshot:

| # | Action | Description |
|---|--------|-------------|
| 21 | Delete file(s) | SaveAttachmentsFolder "work-in-progress.docx" |
| 22 | Save Excel | Save the Excel document stored into ExcelInstance |
| 23 | Close Excel | Save the Excel document and close the Excel instance ExcelInstance |

Figure 1.9 – Housekeeping PAD actions

In detail, these actions do the following:

- **Delete file(s):** Microsoft Word creates a hidden file when it opens a document that sometimes isn’t deleted and stays in the filesystem. To make sure that we will have valid documents in our folder the next time we run the flow, we just delete the hidden file, if it exists.
- **Save Excel:** Saves our entries in the workbook.
- **Close Excel:** Closes the program.

This UI flow might look quite extensive because of multiple repetitions to store data in Excel, but it is nevertheless easy to create and very powerful. The time saved is enormous and it increases the more that parents need to be contacted and the more messages that need to be processed. In addition, this process could also be extended to using physical letters as input or responding to email messages.

There are, of course, other examples of Desktop Flows and Power Automate that create connections to some external systems or UI flows that get triggered from some cloud system. We will see examples of this throughout this book.

## Summary

In this chapter, we started by providing an overview of PAD and how this product has been integrated by Microsoft into their Power Platform ecosystem. We saw that the product fits perfectly into Microsoft’s strategy and fills the gap in desktop software integration very well.

In our first example, we learned how different Office programs can be integrated to automate a repetitive process. Instead of handling each message and document separately, PAD saved the documents and extracted the relevant information. Thereby, we were able to save a lot of time by automating this process.

In the next chapter, we will create a flow by taking a small and very useful tool to help us: the recorder.

## Further reading

To learn more about the topics that were covered in this chapter, take a look at the following resources:

- *Workflow Automation with Microsoft Power Automate*, by Aaron Guilmette (Packt Pub)
- *Microsoft Power Platform Solution Architect’s Handbook*, by Hugo Herrera (Packt Pub)
- Microsoft Power Platform documentation:
  https://learn.microsoft.com/en-us/power-platform/
- About the on-premises data gateway: https://learn.microsoft.com/en-us/power-platform/admin/wp-onpremises-gateway
- Power Platform environments overview:
  https://learn.microsoft.com/en-us/power-platform/admin/environments-overview

## Using Power Automate Desktop and Creating Our First Flow

In the previous chapter, we saw how **Power Automate Desktop (PAD)** was born and how powerful the tool can be for automating repetitive tasks. In this chapter, we will take a detailed look into the following:

- What is necessary for installing PAD on a local machine
- How the application is built up
- How to create our first flow by using the built-in recorder

To use PAD effectively, a basic understanding of the application is essential, as we will spend most of our time on the very areas presented here. Also, knowledge of the PAD recorder will be extremely helpful throughout this book because it can also save us a lot of time when creating flows. Now, let’s look at how we can get PAD running on our computers.

### Understanding the prerequisites for installing PAD

Let’s begin by taking a look at what you need to get Power Automate up and running on a local machine. Two things are required to install Power Automate:

- A Windows operating system (Windows 10 or Windows 11 Home edition or above) and some hardware requirements/recommendations
- A Microsoft work or school account to sign in to Power Automate

Since there are specific things to consider, let’s take a closer look at each of these topics now.

### Microsoft account

To be able to access the files for installing PAD, a Microsoft account (formerly known as Windows Live ID) is required. There are different types of accounts and all of them are suitable for using PAD:

- **(Personal) Microsoft account:** This is probably the most commonly used type of account in the private area. It is free and provides access to Microsoft’s consumer services such as **Outlook.com, OneDrive, and Office applications**. To sign up, an existing email address can be used or a new one can be created. This account can also be used to sign in to a Windows machine.
- **Work or school account:** This type of account is created by an organization that uses **Azure Active Directory (AAD)** as the authentication and authorization platform. Typically, additional services or subscriptions are purchased that offer additional functionality to users.
- **Organization premium account:** This is a special form of work or school account. In this case, the organization is not only managing users via AAD but has also moved certain or all parts of IT to the cloud, such as Exchange, Teams, and Business applications.

With all three types, the full potential of PAD can be unleashed. The organization’s premium account has the following capabilities on top:

- Connecting and integrating with cloud flows (see the previous comments on the operating system)
- Collaboration and advanced functionality (such as central Dataverse storage, sharing of flows, as well as centralized management and reporting)
- Additional premium functionality such as access to AI Builder and unattended execution of flows

For the vast majority of use cases, a personal Microsoft account is sufficient. Nevertheless, in the following chapters of this book, we will see scenarios where a premium account is necessary to access specific services.

### Additional notes on accounts

To install PAD, a user with administrator privileges on the device is required. A Windows local or domain account with these privileges should be used. To access PAD installation files, however, one of the Microsoft accounts mentioned previously is necessary. The Windows local account can be associated with the Microsoft account in the corresponding Windows account settings.

Now that we know the prerequisites, let’s take a look at how to install the software.

### Installing PAD

The installation files for PAD are not on the local Windows medium – you must download these from the Microsoft download center. Therefore, an active internet connection and a modern browser are required – Microsoft Edge version 80 or higher or Google Chrome. For this book, I am using a machine with Windows 11 Home edition and a freshly created personal Microsoft account. To install PAD, do the following:

- 1. Open your browser and navigate to https://www.office.com. If you are not already logged in, do so with your Microsoft account.
- 2. On the right-hand side, you should see a list of familiar icons such as those for Word, Excel, and more. Click on the last one, called Apps. Clicking this icon will make additional apps appear:
