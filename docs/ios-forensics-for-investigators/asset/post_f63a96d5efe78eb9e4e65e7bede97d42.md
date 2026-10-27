
While the last two files don't pose any kind of concern as they are essentially text files and, as such, can be viewed with pretty much any text editor, the others may be (and often are) stored in **binary format**, so they require specific tools or libraries to parse through the data.

## Introducing SQLite

Almost every application on an iOS device, including system ones like **Messages**, **Contacts**, or **Email**, needs a place to store data for the long term. This is achieved by using **SQLite**, which is an open source, small, self-contained relational database. SQLite databases can be recognized by the `.sqlite` or `.sqlite3` file extensions, although some databases are given the `.db` extension, or other extensions as well.

The reason why SQLite is so popular among mobile devices is due to its simple architecture and great performance. The following are some of its features:

- **SQLite is self-contained:** The entire database is contained within a single file that integrates directly with applications. Its serverless architecture means that the database works as a standalone file, as opposed to the traditional client-server protocol.
- **SQLite is cross-platform:** The database resides in a single, cross-platform file that can be accessed by all operating systems supported by SQLite, including Windows, macOS, Linux, Android, iOS, BSD, Solaris, and so on.
- **SQLite is fully ACID-compliant:** This allows safe access to multiple applications or threads. It also supports most of the SQL query language, which is the main means of interacting with nearly all modern relational database systems.

Now that we've seen SQLite's features, let's take a look at how it stores data.

## Tables, columns, and rows

The basic concepts of an SQLite database should be easy to visualize – a database contains one or more **tables**, which each contain data that is organized into **columns** and **rows**. A column represents a single data element, such as a *phone number* or *address*, while a row represents a set of values, one for each column.

SQLite supports five different data types:

- INTEGER
- TEXT
- REAL
- BLOB
- NULL

The first three types are self-explanatory. The **Binary Large Object (BLOB)** data type preserves any input exactly as is, without casting it as a certain type; BLOB data types are commonly used to store complex binary data structures such as **property lists** or **protocol buffers** in an SQLite record. The NULL data type simply stores an empty value.

To retrieve data from a database, you run a **query**, which is a set of logical instructions written in **SQL language** that parses through a given table to extract the data you want. The result of the query is actually represented as another table, as you can see in the following screenshot.

To understand what a query looks like, let's see a simple example. The following query extracts the last five messages from the sms.db database:

```
SELECT ROWID, text FROM message
ORDER BY ROWID DESC
LIMIT 5;
```

The output can be seen in the following screenshot:

![](img/0b17a17d4cb56b1c25763a396dd83117_115_0.png)

Figure 4.1 – The result of a query displayed in DB Browser for SQLite

Without going into too much detail, we will focus on this query for a moment to understand the basics of SQL language. Obviously, if you're doing a forensic examination of an SQLite database, you do not want to modify the data in any way. Therefore, all queries will probably start with the SELECT clause, which indicates that we want to read data from a table.

On the first line, we select which columns to include in the result (ROWID and text) and the name of the table – in this case, the message table. The second line of the query indicates the order in which the results should be displayed. In the example table, each SMS message is identified by a progressive ROWID column, so to display the most recent messages, we're ordering the results in descending order based on this column. As we're only interested in five messages, we use the LIMIT command to indicate how many rows should be displayed.

### Tip

If you're new to SQL language, you can learn how to write the most common types of queries by visiting the SQLite website at

https://www.sqlite.org/lang.html.

As a forensic examiner, the queries you run usually entail filtering and searching through the data, so you may want to look into commands such as CASE, WHEN, and WHERE. We'll see more complex queries over the course of the chapter.

## Running SQL queries

Most forensic tools come with a built-in database viewer that allows the investigator to view the entire database and run queries directly within the tool itself; there are, however, other options:

- **The SQLite command-line utility**, which can be downloaded from http://www.sqlite.org, allows direct interaction with the database from the command line.
- Other free tools, such as **DB Browser for SQLite**, allow the user to view the database through a **Graphical User Interface (GUI)** and run queries on it. DB Browser for SQLite can be downloaded from https://sqlitebrowser.org.

The investigator should be familiar with the tools used to access the database directly, as these will prove to be huge timesavers when the scope of the examination is limited to one or more SQLite databases. Analyzing these databases with forensic tools such as Cellebrite Physical Analyzer or Magnet AXIOM results in the need to load and parse the entire extraction into memory; this can be very time-consuming, depending on the size of the data extracted.

## Using the SQLite command-line tool

sqlite3 is a command-line utility that is part of the SQLite library. macOS and most Linux distributions have it already installed, while Windows users can download the executable from SQLite's website, which is listed in the preceding section.

To open an SQLite database from the command line, just type the following:

```
sqlite3 filename.db
```

If a database with the specified name doesn't exist, SQLite will create it for you. Once you've opened a database, a SQL prompt will be displayed, where you can run queries and commands. There are a number of built-in commands you can issue to obtain basic information on the database and its tables.

If you're not familiar with the database layout, you can list all the tables in it by running the .tables command:

```
sqlite> .tables
attachment                 message
chat                       message_attachment_join
chat_handle_join           message_processing_task
chat_message_join          sync_deleted_attachments
deleted_messages           sync_deleted_chats
```

If you're interested in a specific table, type .schema followed by the name of the table to display the columns and data types of the table:

```
sqlite> .schema handle
CREATE TABLE handle (ROWID INTEGER PRIMARY KEY AUTOINCREMENT UNIQUE, id TEXT NOT NULL, country TEXT, service TEXT NOT NULL);
```

By looking at the column names and types, you should be able to get a basic understanding of what data is stored in the table. In this example, every row in the handle table has an auto-incrementing ROWID column and three columns of TEXT type: id, country, and service.

If you want to retrieve data from the table, you use a SELECT query:

```
SELECT * FROM handle;
```

When you are finished, type `.exit` or `.quit` to terminate the SQLite session.

## Pages, vacuuming, and write-ahead logs

One of the features of SQLite that makes it so interesting to forensic examiners is the possibility of recovering **deleted records** from a database. Depending on the scope of the database, this could mean recovering deleted text messages, browser history, or logs relating to device activity. To have a better understanding of how data recovery is possible, we need to take a step back and learn how SQLite manages `write` and `delete` operations under the hood.

We've already learned that, from a logical point of view, the data in a database is organized into tables that each contain columns and rows. This is a high-level abstraction layer that allows users to interact with a database with a simple data model that is easy to visualize, such as columns and rows; however, at a lower level, the SQLite library actually organizes data differently on the filesystem, storing data into **pages**. Pages are similar to blocks in a filesystem, and their purpose is to store data in binary format. Each page in an SQLite database can have a variable size, with the default size being 4096 bytes, and each page has a distinct number. The first page contains a 100-byte header, which can be analyzed by using a **hex viewer**, such as the hexdump command-line utility available on macOS and Linux.

For example, to view the header from an SQLite database called `sms.db`, we would execute the following command:

```
```bash
hexdump -n 100 -C sms.db
```
```

The `-n` argument specifies the number of bytes that should be displayed. As SQLite has a fixed-size header, we're only interested in the first 100 bytes. The `-C` argument tells the hexdump utility to also display data in string format:
