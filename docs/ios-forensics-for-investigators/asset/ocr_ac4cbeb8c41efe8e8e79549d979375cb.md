Now that we know where email artifacts are located, we're going to jump right into these SQLite databases to parse through email metadata and its content.

# Extracting email metadata

We're going to start by examining the Envelope Index database. The first table of interest is mailboxes. As the name suggests, this table stores a list of all mailboxes configured on the device. This includes different folders for a single account, such as Important, Spam, Drafts, and Trash.

The following screenshot shows some sample data from the mailboxes table:

![](bbox=[0.13, 0.4, 0.86, 0.73])

Figure 8.1 – The mailboxes table displayed in DB Browser

As you can see from the screenshot, each account is identified by a Universally Unique Identifier (UUID) (in this example, 4FD35256-CE13-47FE-9840-EBEB5B9FD9C1) while each folder is identified by a unique ROWID. By analyzing this table, the examiner can gain a rough idea of how many, and which, email accounts are configured on the device.