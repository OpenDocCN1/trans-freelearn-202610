If each column in the spreadsheet represents a column in the database, then we can simply add the structural components of a SQL INSERT command (quoted strings, parenthesis, commands, and line-terminating semicolons) around the columns in the spreadsheet and concatenate the result together into a giant string of INSERT commands.

![](bbox=[0.17, 0.24, 0.83, 0.33])

After using the concatenate (A1:I1) function to attach all the strings in columns A:I, we end up with INSERT statements that look like this:

```
INSERT INTO freenode_topics (dateOfTopic, channel, num_users, message) VALUES('2014-09-19 14:10:47', '#eurovision',4, 'Congratulations to Austria, winner of the Eurovision Song Contest 2014!');
```

These can be pasted into a user-friendly frontend, such as PhpMyAdmin or MySQL Workbench. Or, you can save this as a text file (using your text editor), one INSERT statement after the other. I called my file inserts.sql. This file can now be imported into the database using the command line and MySQL client, as follows:

```
$mysql -uusername -p -hhostname databasename < inserts.sql
```

Or, it can be imported using the source command in the MySQL command-line client like this:

```
$mysql -uusername -p -hhostname
[enter your password]
> use databasename;
> source inserts.sql
```

Either one of these will work to get the data into MySQL. If the script is small enough, you can also use one of the graphical clients, such as MySQL Workbench. Be careful of loading very large scripts into the graphical client, however, as the amount of memory on your client machine may not be sufficient to load hundreds of gigabytes of SQL. I prefer the second method (source) because it prints out a success message following each successful insert, so I know my commands are good.