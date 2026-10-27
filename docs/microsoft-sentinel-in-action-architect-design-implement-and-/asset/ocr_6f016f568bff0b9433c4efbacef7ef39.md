Again, this is just a brief overview of the commands available to you when using KQL. There are a lot more commands you can use, including some that add machine learning capabilities to your queries.

> **Note**
> For a complete list of all the KQL commands and more examples, go to https://docs.microsoft.com/en-us/azure/kusto/query/index.

Let's start looking at the various commands in more detail in the following sections.

# Tabular operators

A tabular operator in KQL is one that can produce data in a mixture of tables and rows. Each tabular operator can pass its results from one command to another using the pipe delimiter. You will see many examples of this throughout this chapter.

# The print command

The print command will print the results of a simple query. You will not be using this in your KQL queries, but it is useful for trying to figure out how commands work and what output to expect.

A simple print command such as print "test" will return a single row with the text test on it.

# The search command

As we stated earlier, to search using KQL, you just need enter the table name in the editor, rather than in SQL where you use the select command. This will return all the columns and all the rows of the table, up to the maximum your application will return.

So, in the ADX, if you enter StormEvents in the search window and click Run, you will see a window like the one shown in the following screenshot. Note that there are a lot more rows available; the following screenshot just shows a small sample: