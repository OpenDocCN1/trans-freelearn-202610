```
log = new Logger('DataMonitor');
Logger.root.level = Level.ALL;
Logger.root.onRecord.listen((LogRecord rec) {
  var entry = '${rec.level.name}\t${rec.time}\t\t${rec.message}\n';
  File logStore = new File("datamonlog.txt");
  logStore.writeAsStringSync(entry, mode: FileMode.APPEND);
});
log.info('Earthquake Monitor - Data fetcher. Starting...');
}
```

The logging is set up in bin/main.dart and the output appended to a text file. Note that this is not saved to the data sub-folder, as it may get lost with all the data files.

In the fetchData method of the DataMonitor class, the Logger object is set up in the constructor, and key events will be noted. The different levels of logging are used (info, fine, severe, and so on), which would allow an operator of the program to adjust the level of logging to the desired level.

# Saving to the database

The data collector application currently writes to the filesystem. While this is useful for archiving, it makes it difficult and slow to access and query any sort of analysis or display.

![](bbox=[0.24, 0.6, 0.3, 0.66])

> If you are not experienced with databases, here is a very rapid introduction! Databases are collections of data in a structured form. These are accessed and managed via a database management system. You may have heard of SQL Server, MySQL, Hadoop or Oracle.
> Databases are typically accessed using a language called SQL to read and write records from the database. Data is usually organized into tables, and each table is made up of records. Each table has a set columns defined as data types—it is similar to variables in a programming language. SQL is strong on sorting, grouping, and working with sets of data.
> SQL can be stored in the database in a number of forms, with the most common being a stored procedure that can add/remove and update data, and perform some processing.
> Connecting to a database almost always requires a login and may be on a remote computer. This is sometimes referred to as a connection string. It behaves much like a network connection.