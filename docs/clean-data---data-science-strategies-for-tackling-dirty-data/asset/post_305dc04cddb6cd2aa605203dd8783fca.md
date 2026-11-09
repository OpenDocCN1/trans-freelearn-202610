
## Other data types

Numbers, dates/times, and strings are the big three data types, but there are many other types of exotic data that we will run into depending on what environment we are working in. Here is a quick rundown of some of the more interesting ones:

- **Sets/enums**: If your data seems to only have a few choices for possible data values, you may be looking at a set or enumerated (enum) type. An example of enumerated data might be the set of possible final grades in a college course: {A, A-, B+, B, B-, C+, C, C-, D+, D, D-, F, W}.
- **Booleans**: If your data is limited to one of just two selections, and they evaluate to 0/1 or true/false, you may be looking at Boolean data. A database column called package_shipped might have values of either yes or no, indicating whether a package has been shipped out.
- **Blobs (binary large objects)**: If your data is binary, for example, you are storing the actual bytes for a picture file (not just the link to it) or the actual bytes for an mp3 music file, then you are probably looking at blob data.

## Converting between data types

Conversions between data types are an inevitable part of the cleaning job. You may be given string data, but you know that you need to perform math on it, so you wish to store it as a number. You may be given a date string in one format but you want it in a different date format. Before we go any further, though, we need to discuss a potential problem with conversions.

## Data loss

There is a risk of losing data when converting from one data type to another. Usually, this happens when the target data type is not capable of storing as much detail as the original data type. Sometimes, we are not upset by data loss (in fact, there may even be times when a cleaning procedure will warrant some intentional loss), but if we are not expecting the data loss, it can be devastating. Risk factors include:

- **Converting between different sizes of the same data type**: Suppose you have a string column that is 200 characters long and you decide to move the data to a column that is only 100 characters long. Any data that exceeds 100 characters might be chopped off or truncated. This can also happen when switching between different sizes of numeric columns, for example, when going from big integers to regular integers or from regular integers to tiny integers.
- **Converting between different levels of precision:** Suppose you have a list of decimal numbers that are precise to four digits, and then you convert them to two digits of precision, or even worse, to integers. Each number will be rounded off or truncated, and you will lose the precision that you had.

## Strategies for conversion

There are many strategies to handle data type conversions, and which one you use depends on where the data is being stored at the moment. We are going to cover two of the most common ways in which we encounter data type conversions in the data science cleaning process.

The first strategy, SQL-based manipulation, will apply if we have data in a database. We can use database functions, available in nearly every database system, to slice and dice the data into a different format, suitable for either exporting as a query result or for storing in another column.

The second strategy, file-based manipulation, comes into play when we have been given a flat file of data – for example, a spreadsheet or JSON file – and we need to manipulate the data types in some way after reading them out of the file.

## Type conversion at the SQL level

Here, we will walk through a few common cases when SQL can be used to manipulate data types.

### Example one – parsing MySQL date into a formatted string

For this example, we will return to the Enron e-mail dataset we used in Chapter 1, Why Do You Need Clean Data?. As with the previous example, we are going to look in the message table, where we have been using the date column, which is stored as a datetime MySQL data type. Suppose we want to print a full date with spelled-out months (as opposed to numbers) and even the day of the week and time of day. How can we best achieve this?

For the record with the message ID (mid) of 52, we have:

```
2000-01-21 04:51:00
```

What we want is this:

```
4:51am, Friday, January 21, 2000
```

- Option 1: Use concat() and individual date and time functions, as shown in the following code sample. The weakness of this option is that the a.m./p.m. is not printed easily:

```
SELECT concat(hour(date),
':',
minute(date),
', ',
dayname(date),
', ',
monthname(date),
' ',
day(date),
', ',
year(date))
FROM message
WHERE mid=52;
```

Result:

```
4:51, Friday, January 21, 2000
```

If we decided that we really wanted the a.m./p.m., we could use an if statement that tests the hour and prints "a.m." if the hour is less than 12 and "pm" otherwise:

```
SELECT concat(
    concat(hour(date),
        ': ',
        minute(date)),
    if(hour(date)<12,'am','pm'),
    concat(
        ', ',
        dayname(date),
        ', ',
        monthname(date),
        ' ',
        day(date),
        ', ',
        year(date)
)
)
FROM message
WHERE mid=52;
```

Result:

4:51am Friday, January 21, 2000

![](img/9aa003afabe05fdb07da46ad71325de6_61_0.png)

> MySQL date and time functions, such as day() and year(), are described in their documentation: http://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html, and their string functions, such as concat(), can be found here: http://dev.mysql.com/doc/refman/5.7/en/string-functions.html. Other database management systems will have similar versions of these functions.

- Option 2: Use the more sophisticated date_format() MySQL function. This function takes a series of string specifiers for how you want the date to be formatted. There is a very long list of these specifiers in the MySQL documentation. A completed example to convert the date into our desired format is shown in the following code:

```
SELECT date_format(date, '%l:%i%p, %W, %M %e, %Y')
FROM message
WHERE mid=52;
```

Result:

4:51AM, Friday, January 21, 2000

This is pretty close to what we said we wanted, and it is much shorter than Option 1. The only difference is that the a.m./p.m. is capitalized. If we really want it lowercased, we can do this:

```
SELECT concat(
    date_format(date, '%l:%i'),
    lower(date_format(date,'%p ')),
    date_format(date,'%W, %M %e, %Y')
)
FROM message
WHERE mid=52;
```

Result:

4:51am, Friday, January 21, 2000

### Example two – converting a string into MySQL's date type

For this example, let's look at a new table in the Enron schema: the table called `referenceinfo`. This table shows the messages to which the other messages refer. For example, the first entry in that table, with a `rfid` 2, contains the text of the e-mail to which message 79 refers. The column is a string, and its data looks like this (in part):

```
> From: Le Vine, Debi> Sent: Thursday, August 17, 2000 6:29 PM> To: ISO Market Participants> Subject: Request for Bids - Contract for Generation Under Gas> Curtailment Conditions>> Attached is a Request for Bids to supply the California ISO with> Generation
```

This is a very messy string! Let's take on the job of extracting the date shown on the top line and converting it into a MySQL date type, suitable for inserting into another table or for performing some date math.

To do this, we will use the built-in `str_to_date()` MySQL function. This function is a bit like `date_format()` that we saw earlier, except that it's backwards. Here is a working example that will look for the word `Sent:` and extract the following characters up to the `>` symbol and then turn these characters into a real MySQL datetime data type:

```
SELECT
str_to_date(
  substring_index(
    substring_index(reference,' >',3),
    'Sent: ',
    -1
  ),
  '%W,%M %e, %Y %h:%i %p'
)
FROM referenceinfo
WHERE mid=79;
```

Result:

```
2000-08-17 18:29:00
```

Now we have a datetime value that is ready for inserting into a new MySQL column or for performing more date functions or calculations on it.
