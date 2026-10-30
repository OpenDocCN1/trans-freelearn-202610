Figure 11.5 – Execute SQL statement

The following comments can be made about the SQL statement:

+   - Very often, the quality of the statement is crucial for the performance of the database and, thus, the entire system. Therefore, queries such as `SELECT * FROM ...` should be avoided if possible.
+   - Of course, other content from previous flow steps that are stored in variables can now be inserted into the statement.

In the next main section, we will look at an example of how a database can be integrated into a larger context. Before that, let’s take a look at the other services in this category.

# Using local exchange and other email services

We have already learned in *Chapter 2* and *Chapter 8* how to work with a locally installed Outlook application and the corresponding PAD actions to receive, process, and send emails. In addition to this possibility, PAD provides other action groups to work with emails. These behave very similarly to what we have already learned about before, except for the connection setup. These action groups are **Exchange Server** and **Email**. To provide an overview and to see the difference between those action groups, including the one for **Outlook**, let’s look at the following table.

| Feature/Action group | Outlook | Exchange Server | Email |
|---|---|---|---|
| Connection through | Local Outlook client | Local/on-premises Exchange Server deployment | Any **Internet Message Access Protocol (IMAP)** mailbox |